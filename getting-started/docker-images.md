# Docker Images

Terrakube publishes four images to Docker Hub. All are multi-architecture (`linux/amd64` and `linux/arm64`).

| Component | Image |
| --- | --- |
| Terrakube API | [`azbuilder/api-server`](https://hub.docker.com/r/azbuilder/api-server/tags) |
| Terrakube Executor | [`azbuilder/executor`](https://hub.docker.com/r/azbuilder/executor/tags) |
| Terrakube Registry | [`azbuilder/open-registry`](https://hub.docker.com/r/azbuilder/open-registry/tags) |
| Terrakube UI | [`azbuilder/terrakube-ui`](https://hub.docker.com/r/azbuilder/terrakube-ui/tags) |

### Tags

| Tag | Meaning |
| --- | --- |
| `2.33.3` | A release. Use an exact version in production. |
| `2.34.0-beta.10` | A pre-release of the next version. |
| `2.33.3-alpaquita` | The same release on Alpaquita Linux (API, executor and registry only). |
| `latest` | The newest build, which can be a pre-release. Do not use it in production. |

Run the API, executor, registry and UI at the **same version**. With the Helm chart set `api.version`, `executor.version`, `registry.version` and `ui.version`; see [Helm Chart](deployment/helm-chart.md#choose-the-terrakube-version) and [Upgrading](upgrading.md). To reproduce a deployment exactly, pin the image digest as well as the tag.

```bash
docker pull azbuilder/api-server:2.33.3
docker pull azbuilder/executor:2.33.3
docker pull azbuilder/open-registry:2.33.3
docker pull azbuilder/terrakube-ui:2.33.3
```

### How they are built

The default images are based on Ubuntu Jammy and are built with [Cloud Native Buildpacks](https://buildpacks.io/) through the Spring Boot Maven plugin, which is also how [custom CA certificates](deployment/custom-ca-certs.md) are added at runtime. The release pipeline also generates software bills of materials and pins base images by digest; see [Security](security.md).

### Alpaquita Linux (experimental)

Every release also has API, executor and registry images based on [Alpaquita Linux](https://bell-sw.com/alpaquita-linux/), a smaller image built for Spring Boot. They use the same version with an `-alpaquita` suffix:

```bash
docker pull azbuilder/api-server:2.33.3-alpaquita
docker pull azbuilder/open-registry:2.33.3-alpaquita
docker pull azbuilder/executor:2.33.3-alpaquita
```

{% hint style="danger" %}
The Alpaquita **executor** image does not include Git, so Terraform modules that clone over Git during a run will not work, for example:

```hcl
module "consul" {
  source = "git@github.com:hashicorp/example.git"
}
```
{% endhint %}

### Use your own registry

To pull from a private mirror, copy the images and point the chart at them with `api.image`, `executor.image`, `registry.image` and `ui.image`, and add `global.imagePullSecrets` if the mirror needs credentials.
