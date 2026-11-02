# Production Setup

Follow this order to deploy Terrakube and configure access before the first production run. If you only want to try it locally, use [Your First Run](first-run.md).

### 1. Choose your deployment workflow and controls

Start with [Recommended Settings](recommended-settings.md). Decide whether production changes come from a protected Git branch, exact-commit CI, or CLI uploads. This determines which workspace restrictions to enable later.

Choose token lifetimes and the teams that may plan, approve and destroy. You will apply service settings during deployment and workspace settings after creating each workspace.

### 2. Deploy and configure the services

Follow [Deployment](deployment/README.md) and the [Helm Chart](deployment/helm-chart.md). Configure ingress/TLS, storage, the database and Redis, then connect [user authentication](user-management/README.md).

Apply the service recommendations in [Token Security](deployment/token-security.md): independent signing secrets, token issuance/lifetime controls and, after verifying your Dex audiences, audience validation. These go in the API and registry deployment environment, not in workspace variables. Use the [Configuration Reference](../user-guide/reference/configuration/README.md) to look up exact names and defaults.

### 3. Create an organization and assign team access

[Create an organization](../user-guide/organizations/organizations.md), then [create teams](../user-guide/organizations/team-management.md) that match your identity-provider groups. Assign only the permissions each team needs; review destroy access on Custom roles.

Create [projects](../user-guide/projects/README.md) if you want to group workspaces and grant [project-level access](../user-guide/projects/team-access.md). Permissions from organization, project and workspace access are additive.

### 4. Prepare the source, templates and automation identity

Connect a [VCS provider](../user-guide/vcs-providers/README.md) for repository-driven workspaces. Prepare [templates](../user-guide/organizations/templates/README.md), including a plan-only template for PRs and a deployment template with an explicit approval team and saved-plan requirement.

For CI, configure [Federated Identity](../user-guide/organizations/federated-identity.md) with narrow claim conditions. For exact-commit deployments, follow the [GitHub Actions](../user-guide/ci-cd-integration/github-actions.md) guide and its branch-restriction compatibility note. CLI-driven workspaces instead use the organization's [CLI templates](../user-guide/organizations/templates/default-templates.md#cli-driven-templates).

### 5. Create and configure each workspace

[Create a workspace](../user-guide/workspaces/creating-workspaces.md), select its project and repository/branch or CLI workflow, then configure:

| Configure | Where | Guide |
| --- | --- | --- |
| Scoped team access | Workspace **Settings > Team Access** | [Team Access](../user-guide/workspaces/team-access.md) |
| Terraform inputs and environment values | Workspace **Variables** | [Variables](../user-guide/workspaces/variables.md) |
| Cloud authentication | Workspace variables and the cloud provider's trust configuration | [Dynamic Provider Credentials](../user-guide/workspaces/dynamic-provider-credentials/README.md) |
| Branch and ungated-apply controls | Workspace **Settings > General**, as an organization-level workspace manager | [Run Restrictions](../user-guide/workspaces/run-restrictions.md) |

Apply the choices from step 1. `restrictApplyToConfiguredBranch` and `requireApproveToApply` are workspace attributes, not service environment variables or Terraform inputs. Branch restriction is unavailable in the UI for CLI-driven workspaces and conflicts with API-created exact-commit apply runs.

### 6. Validate a run before adding automatic triggers

[Start a run](../user-guide/workspaces/starting-a-run.md) with the intended planning identity. Check the branch, template, cloud access and approval team. Confirm the permission boundaries with a disposable workspace before using production resources.

Then configure [webhooks](../user-guide/workspaces/webhooks.md), [schedules](../user-guide/workspaces/workspace-scheduler.md) or [run triggers](../user-guide/workspaces/run-triggers.md), and test those paths too. A successful manual run does not establish that a PR or CI run uses the intended identity and template.

### 7. Operate and upgrade

Configure [executor output and run limits](deployment/executor-log-output.md), [notifications](../user-guide/workspaces/notifications.md) and backups. Use [Upgrading](upgrading.md) and [Updates](../updates.md) before changing versions.

Optional features such as the [HTTP state backend](deployment/http-state-backend.md), [plan artifacts](../user-guide/workspaces/plan-artifacts.md) and [policy governance](../user-guide/policy-enforcement-opa.md) have their own configuration guides. Add them when the workflow needs them.
