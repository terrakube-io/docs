---
description: 'Images for Terrakube components can be found in the following links:'
---

# Docker Images

| Component          | Link                                                                                                           |
| ------------------ | -------------------------------------------------------------------------------------------------------------- |
| Terrakube API      | [https://hub.docker.com/r/terrakube-io/api-server/tags](https://hub.docker.com/r/terrakube-io/api-server/tags)       |
| Terrakube Registry | [https://hub.docker.com/r/terrakube-io/open-registry/tags](https://hub.docker.com/r/terrakube-io/open-registry/tags) |
| Terrakube Executor | [https://hub.docker.com/r/terrakube-io/executor/tags](https://hub.docker.com/r/terrakube-io/executor/tags)           |
| Terrakube UI       | [https://hub.docker.com/r/terrakube-io/terrakube-ui/tags](https://hub.docker.com/r/terrakube-io/terrakube-ui/tags)   |

{% hint style="success" %}
Default Images are based on Ubuntu Jammy (linux/amd64)
{% endhint %}

The docker images are generated using cloud native buildpacks with the maven plugin, more information can be found in these links:

* [https://docs.spring.io/spring-boot/docs/current/reference/html/container-images.html#container-images.buildpacks](https://docs.spring.io/spring-boot/docs/current/reference/html/container-images.html#container-images.buildpacks)
* [https://docs.spring.io/spring-boot/docs/3.1.5/maven-plugin/reference/htmlsingle/#build-image](https://docs.spring.io/spring-boot/docs/3.1.5/maven-plugin/reference/htmlsingle/#build-image)

### Alpaquita Linux (Experimental)

From Terrakube 2.17.0 we generate 3 additional  docker images for the API, Registry and Executor components that are based on [Alpaquita Linux](https://bell-sw.com/alpaquita-linux/) that are designed and optimized for Spring Boot applications.

```
docker pull terrakube-io/api-server:2.17.0-alpaquita
docker pull terrakube-io/open-registry:2.17.0-alpaquita
docker pull terrakube-io/executor:2.17.0-alpaquita
```

{% hint style="danger" %}
Alpaquita Linux images for the Executor component does not include GIT, Terraform modules using remote executions like the following wont work:

```
module "consul" {
  source = "git@github.com:hashicorp/example.git"
}
```
{% endhint %}
