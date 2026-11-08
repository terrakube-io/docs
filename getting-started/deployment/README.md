# Deployment

The supported way to run Terrakube in production is the [Helm chart](helm-chart.md). For the complete order from choosing settings to running a workspace, follow [Production Setup](../production-setup.md). This section covers deploying the services.

### Deployment order

1. Read [Recommended Settings](../recommended-settings.md) and choose the service controls you will apply.
2. Follow [Helm Chart](helm-chart.md) to configure storage, database/Redis, ingress and authentication before deployment.
3. Apply [Token Security](token-security.md) settings to the API and registry; verify sign-in and registry access.
4. Continue with [organizations and teams](../production-setup.md#3-create-an-organization-and-assign-team-access), then configure workspace settings. Deployment alone does not enable workspace run restrictions.

### Deployment topics

| I want to... | Go to |
| --- | --- |
| Try it on a laptop | [Docker Compose](../docker-compose.md), [Minikube](minikube.md) or [Minikube + HTTPS](minikube-+-https.md) |
| Install on a cluster | [Helm Chart](helm-chart.md) |
| Expose it with a domain and TLS | [Ingress Configuration](ingress-configuration.md) |
| Let users sign in | [User Authentication (Dex)](user-authentication-dex.md), [User Management](../user-management/README.md) |
| Store state, logs and modules | [Storage backend](storage-backend/README.md) |
| Use my own database or Redis | [Database backend](database-backend/README.md), [External Redis](external-redis.md) |
| Run jobs on separate or per-run executors | [Self-Hosted Agents](self-hosted-agents.md), [Ephemeral Agents](ephemeral-agents.md) |
| Fit it into my network | [Custom CA Certs](custom-ca-certs.md), [Proxy Configuration](proxy-configuration.md), [Custom Terraform CLI Builds](custom-terraform-cli-builds.md) |
| Choose production controls | [Recommended Settings](../recommended-settings.md) |
| Route executor state through the API | [Executor HTTP State Backend](http-state-backend.md) |
| Look up a setting | [Configuration Reference](../../user-guide/reference/configuration/README.md) |
| Upgrade safely | [Upgrading](../upgrading.md) |
| Secure and observe it | [Token Security](token-security.md), [Open Telemetry](open-telemetry.md), [Executor Log Output](executor-log-output.md) |
