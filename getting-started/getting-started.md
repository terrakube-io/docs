# Getting Started

Pick the path that matches what you want to do.

### 1. Try it locally

Run the whole stack on your machine with [Docker Compose](docker-compose.md), or on a laptop Kubernetes cluster with [Minikube + HTTPS](deployment/minikube-+-https.md).

### 2. Run your first workspace

Follow [Your First Run](first-run.md) for a guided walkthrough, or in short:

1. [Create an organization](../user-guide/organizations/organizations.md) and a [team](../user-guide/organizations/team-management.md) that your users belong to.
2. Connect a [VCS provider](../user-guide/vcs-providers/README.md) (or skip this and use the CLI).
3. [Create a workspace](../user-guide/workspaces/creating-workspaces.md), add its [variables](../user-guide/workspaces/variables.md), and [start a run](../user-guide/workspaces/starting-a-run.md).

### 3. Go to production

Follow the [Helm deployment](deployment/README.md), then configure [user authentication](user-management/README.md), a [storage backend](deployment/storage-backend/README.md) and a [database](deployment/database-backend/README.md). Read [Security](security.md) and [Architecture](design-and-architecture.md) before exposing it to your team.

### 4. Integrate

Use Terrakube from [the Terraform CLI](../user-guide/workspaces/cli-driven-workflow.md), [GitHub Actions and other CI/CD](../user-guide/ci-cd-integration/README.md), the [Terrakube CLI](../user-guide/terrakube-cli/README.md) or the [API](../api/getting-started.md).
