# Deployment

The supported way to run Terrakube in production is the [Helm chart](helm-chart.md). Start there: it lists the production checklist and links to each topic below.

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
| Secure and observe it | [Token Security](token-security.md), [Open Telemetry](open-telemetry.md), [Executor Log Output](executor-log-output.md) |
