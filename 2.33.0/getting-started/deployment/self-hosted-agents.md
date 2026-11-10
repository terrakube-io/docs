# Self-Hosted Agents

{% hint style="info" %}
This feature is supported from version 2.20.0 and helm chart version 3.16.0
{% endhint %}

Terrakube allow to have one or multiple agents to run jobs, you can have as many agents as you want for a single organization.

To use this feature you could deploy a single executor component using the following values:

```yaml
# Executor should be enable but we need to customize the apiServiceUrl
executor:
  enabled: true
  apiServiceUrl: "http://terrakube-api-service.terrakube:8080" ## The API is in another namespace called "terrakube"

# We need to disable the default openLdap but we need to provide the internal secret
# so the executor can authenticat with the API and the Registry
security:
  useOpenLDAP: false
  internalSecret: "AxxPdgpCi72f8WhMXCTGhtfMRp6AuBfj"

# We need to disable dex in this deployment
dex:
  enabled: false

# We need to disable default storage MINIO and set some custom values 
# in this example will be deploying like using an external MINIO
#(other backend storage could be used too)
storage:
  defaultStorage: false
  minio:
    accessKey: "admin"
    secretKey: "superadmin"
    bucketName: "terrakube"
    endpoint: "http://terrakube-minio.terrakube:9000" ## MINIO is in another namespace called "terrakube"

# We need to disable API, the default redis and default postgresql database
# But we need to provide some properties like the redis connection
api:
  enabled: false
  defaultRedis: false
  defaultDatabase: false
  properties:
    redisHostname: "terrakube-redis-master.terrakube" ## REDIS is in another namespace called "terrakube"
    redisPassword: "7p9iWVeRV4S944"

# We need to disable registry deployment
registry:
  enabled: false

# We need to disable ui deployment
ui:
  enabled: false

# We need to disable the ingress configuration
# but we need to specify the api and registry URL 
ingress:
  useTls: false
  includeTlsHosts: false
  ui:
    enabled: false
  api:
    enabled: false
    domain: "terrakube-api.minikube.net"
  registry:
    enabled: false
    domain: "terrakube-reg.minikube.net"
  dex:
    enabled: false
```

{% hint style="warning" %}
The above values are assuming the we have deploy terrakube using the domain "minikube.net" inside a namespace called "terrakube"
{% endhint %}

Now that we have our values.yaml we can use the following helm command:

```
helm install --debug --values ./your-values.yaml terrakube terrakube-repo/terrakube -n self-hosted-executor
```

Now we have a single executor component ready to accept jobs, or we can increase `executor.replicaCount` to run a pool of agents:

<figure><img src="../../.gitbook/assets/image (62).png" alt=""><figcaption></figcaption></figure>

### Scaling to multiple replicas

{% hint style="info" %}
Reliable multi-replica scheduling requires **Terrakube 2.33+** and **Helm chart 4.7.7+**. On earlier versions, a Kubernetes `Service` could round-robin an incoming job to a pod that was already mid-run, since each executor pod only ever processes one job at a time.
{% endhint %}

From 2.33 onward, an executor pod publishes `REFUSING_TRAFFIC` on its readiness probe for the duration of a job, so Kubernetes pulls it out of the Service's endpoints as soon as it picks up work, and puts it back once it's free. This makes it safe to run `executor.replicaCount` greater than 1 as a static pool — there's no autoscaling, so size the pool to your expected peak concurrent job count.

Two values control this behavior:

```yaml
executor:
  replicaCount: "3"
  properties:
    # Longest a single job may run before this pod is marked unhealthy (liveness)
    # so Kubernetes restarts it - covers a hung terraform process or a
    # looping hook script.
    maxJobDurationMinutes: "360"
  readinessProbe:
    # Low periodSeconds/failureThreshold so a pod that just started a job is
    # pulled out of the Service's endpoints almost immediately.
    periodSeconds: 2
    failureThreshold: 1
```

Ephemeral executors (see [Ephemeral Agents](ephemeral-agents.md)) skip restoring `ACCEPTING_TRAFFIC` after a job, since the pod is about to be torn down anyway.

{% hint style="info" %}
**Admission control backstops the readiness probe.** Even with fast readiness polling, Kubernetes can still route a second job to a pod before its `REFUSING_TRAFFIC` state has propagated — in practice this showed up as several jobs landing on the same pod within a couple of seconds while other replicas sat idle. A busy persistent executor pod now rejects a second concurrent job outright with `503 Service Unavailable` instead of queuing it locally; the API scheduler treats `503` as retryable and reassigns the job to another pod. This is purely a safety net underneath the readiness-probe behavior above — it doesn't change how many jobs a pod can run at once (still one) or require any configuration.
{% endhint %}

The API dispatcher keeps queued work in first-in, first-out order while it looks for an available executor. If an executor stops responding after a job has been dispatched, Terrakube detects the stale dispatch and returns the job to the queue so another healthy replica can run it. This recovery is automatic; use the existing job-status and executor logs when investigating a repeated failure.
