# Helm Chart

The [Terrakube Helm chart](https://github.com/terrakube-io/terrakube-helm-chart) deploys the API, executor, registry and UI, plus Dex for sign-in, onto any Kubernetes cluster (1.19+) with Helm 3.

```bash
helm repo add terrakube-repo https://charts.terrakube.io
helm repo update
helm search repo terrakube-repo/terrakube --versions | head

kubectl create namespace terrakube
helm install terrakube terrakube-repo/terrakube -n terrakube
```

That command installs a **sandbox**: sample data, a demo OpenLDAP directory, and bundled PostgreSQL, MinIO and Valkey. To try it on a laptop follow [Minikube](minikube.md) or [Minikube + HTTPS](minikube-+-https.md).

{% hint style="danger" %}
The bundled OpenLDAP, PostgreSQL and MinIO use Bitnami images and well-known default passwords and secrets. They are for evaluation only. Use the production checklist below before exposing Terrakube to users.
{% endhint %}

### Choose the Terrakube version

The chart's default image tag is its `appVersion` (chart 4.7.12 ships Terrakube 2.33.1), which can lag the latest release. Pin the versions you want in your values file; they are independent per component and should match:

```yaml
api:
  version: "2.33.3"
executor:
  version: "2.33.3"
registry:
  version: "2.33.3"
ui:
  version: "2.33.3"
```

Images are published to Docker Hub as `azbuilder/api-server`, `azbuilder/executor`, `azbuilder/open-registry` and `azbuilder/terrakube-ui` (see [Docker Images](../docker-images.md)). Pin the chart version too, with `helm install ... --version <chart version>`, so upgrades are deliberate.

### Production checklist

Create a `terrakube.yaml` and install with `helm install --values terrakube.yaml terrakube terrakube-repo/terrakube -n terrakube`.

| Area | What to set | Guide |
| --- | --- | --- |
| Sign-in | A real Dex connector and `security.useOpenLDAP: false`. Set `security.adminGroup` to the group whose members administer Terrakube. | [User Authentication (Dex)](user-authentication-dex.md), [User Management](../user-management/README.md) |
| Secrets | New 32-character `security.patSecret` and `security.internalSecret`. The values in the chart are public. | [Token Security](token-security.md) |
| Database | `api.defaultDatabase: false` and your own PostgreSQL, MySQL or SQL Server. Set `api.loadSampleData: false`. | [Database backend](database-backend/README.md) |
| Redis | `api.defaultRedis: false` and an external Redis or Valkey. Required when you run more than one API or executor replica. | [External Redis](external-redis.md) |
| Storage | `storage.defaultStorage: false` and S3, Azure Blob, Google Cloud Storage or your own MinIO. | [Storage backend](storage-backend/README.md) |
| Ingress and TLS | Your controller, domains and certificates. | [Ingress Configuration](ingress-configuration.md) |
| Executors | Replica count sized to peak concurrent runs (each pod runs one job at a time), or ephemeral Kubernetes jobs. | [Self-Hosted Agents](self-hosted-agents.md), [Ephemeral Agents](ephemeral-agents.md) |
| Observability | OpenTelemetry metrics and traces. | [Open Telemetry](open-telemetry.md) |
| Private CAs and proxies | Custom certificates, or a corporate proxy. | [Custom CA Certs](custom-ca-certs.md), [Proxy Configuration](proxy-configuration.md) |

Any component accepts extra environment variables under `env`, which is how most Terrakube settings in this documentation are applied. Every variable and its default is listed in the [Configuration Reference](../../user-guide/reference/configuration/README.md):

```yaml
api:
  env:
    - name: ModuleRefreshInterval
      value: "60"
```

### Upgrading

```bash
helm repo update
helm upgrade terrakube terrakube-repo/terrakube -n terrakube --values terrakube.yaml --version <chart version>
```

Database migrations run automatically when the API starts. Read [Upgrading](../upgrading.md) before you upgrade: it has the chart and version compatibility table and the breaking changes for each release.

{% hint style="warning" %}
**Upgrading to Terrakube 2.34.0**

* **Executor pool:** set `executor.emptyDirSecret: true` (chart 4.7.12 or later). From 2.34.0 the executor reads its internal secret from a file and removes it after loading, so the file must be in a writable volume. The chart copies the secret into an `emptyDir` with an init container (`executor.initSecretImage`, default `busybox`).
* **Ephemeral executors:** the API now mounts the executor secret into each ephemeral job as files instead of environment variables (`ExecutorEphemeralEmptyDirSecret`, default `true`). The executor image used for ephemeral jobs must therefore be 2.34.0 or later. If you pin an older ephemeral executor image, set `ExecutorEphemeralEmptyDirSecret=false` on the API.

Upgrade the API, executor, registry and UI together.
{% endhint %}
