# 👮 Self-Hosted Agents

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

Now we have a single executor component ready to accept jobs or we could change the number or replicas to have multiple replicas like a pool of agent:

<figure><img src="../../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

