---
layout:
  width: wide
---

# Configuration Reference

Every Terrakube service is configured with environment variables. This section lists all of them, with their defaults, for the three services you deploy:

* [API](api.md): database, storage, authentication, scheduler, Redis, logs, notifications, dynamic credentials, policy governance
* [Executor](executor.md): Terraform and OpenTofu, state and output storage, structured output, log delivery, ephemeral jobs
* [Registry](registry.md): modules, providers, storage, caching

Most topics have a task-oriented guide that explains *why* and *when* to change a setting; this reference is for looking up the exact name and default. Names are case sensitive.

### How to set them

| Deployment | Where |
| --- | --- |
| Helm chart | Under `env` of the component: `api.env`, `executor.env`, `registry.env`. Common settings also have chart values; see [Helm Chart](../../../getting-started/deployment/helm-chart.md). |
| Docker Compose | The `environment` block of the service in `docker-compose.yml`, or `docker-compose/.env`. |
| Plain containers | `-e Name=value`. |

```yaml
api:
  env:
    - name: ModuleRefreshInterval
      value: "60"
```

A variable with no default is either optional or must be provided. Secrets such as `PatSecret`, `InternalSecret` and database or storage credentials have no default and are required where the feature is used.

{% hint style="info" %}
These pages are generated from the `application.properties` of each service, so they always list every variable. The text comes from the comments in those files; a row that only says "Sets `property`" has no comment in the source. Generate them again for a release with `scripts/generate-config-reference.py`.
{% endhint %}
