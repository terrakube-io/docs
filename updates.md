# Updates

### 2.34.0 (upcoming)

#### Policy Governance (OPA)

Platform teams can now enforce Open Policy Agent policies across every run. Policy sets are stored in Git, attached to an organization, project, tag or workspace, and evaluated inside the runner before apply at one of three levels (`hard_mandatory`, `soft_mandatory`, `advisory`). Soft violations can be overridden by a designated team with an audited justification, individual rules can be waived with expiring exemptions, and a nightly job surfaces compliance drift. See [Policy Governance](user-guide/policy-enforcement-opa.md). The previous template-based approach is documented as [legacy](user-guide/policy-enforcement-opa-templates.md).

#### Run Triggers

Workspaces can be chained: when an apply changes the source workspace, a run is queued in each dependent workspace, with loop detection and a dependency view in the UI. See [Run Triggers](user-guide/workspaces/run-triggers.md).

#### Terraform and OpenTofu CLI login

`terraform login` and `tofu login` can authenticate through a Terrakube OAuth broker that issues a revocable personal access token with a lifetime you choose. See [CLI-driven Workflow](user-guide/workspaces/cli-driven-workflow.md#terraform-login).

#### Runs

* Run approval moved to a sticky bar on the run page and to the workspace overview, which now shows the run that needs action. Terrakube records who approved a run. See [Starting a Run](user-guide/workspaces/starting-a-run.md#approve-or-discard-a-run).
* Run-specific Terraform variables can be sent with a job from CI. See [API-driven Workflow](user-guide/workspaces/api-driven-workflow.md).
* Files built by the plan step can be carried to the apply step. See [Plan Artifacts](user-guide/workspaces/plan-artifacts.md).
* The run page gained a Diagnostics panel, output-only plan changes, and an indicator of whether a step's result is final. Structured output is no longer lost when the API is briefly unavailable, and output is archived while a step runs so an executor crash no longer loses the log. See [Job Output](user-guide/workspaces/job-output.md).
* Step output is no longer copied to the executor's service log by default. See [Executor Log Output](getting-started/deployment/executor-log-output.md).
* Ephemeral executor jobs gained deadline, backoff, TTL and grace period controls. See [Ephemeral Agents](getting-started/deployment/ephemeral-agents.md).

#### Workspaces and projects

* Workspaces can have key/value tags, editable in the UI and filterable in the list. See [Tags](user-guide/organizations/tags.md).
* A workspace's repository URL and VCS provider can be changed in **Settings → General**. See [Creating Workspaces](user-guide/workspaces/creating-workspaces.md#changing-the-repository-or-vcs-provider).
* The webhook settings were redesigned with per-pattern validation, priority ordering and shared-webhook controls. See [Webhooks](user-guide/workspaces/webhooks.md).
* A Module Cache page documents sharing `TF_DATA_DIR` between runs. See [Module Cache](user-guide/workspaces/module-cache.md).
* Projects can be searched, workspace lists group by project, and the project workspaces tab shows each workspace's last run.

#### Registry

* Module and provider versions can be deprecated or removed. See [Deprecating and Removing Versions](user-guide/private-registry/deprecating-and-removing-versions.md).
* Public providers open a provider page with the latest version preselected before you add them. See [Using Providers](user-guide/private-registry/using-providers.md).
* Module inputs, outputs and resources are parsed on the server, which makes large modules open faster.
* The module refresh delay is configurable, and the registry can redirect module downloads to presigned S3, Azure Blob or Google Cloud Storage URLs. See [Private Registry](user-guide/private-registry/README.md).

#### Storage and operations

* S3-compatible stores can set the signing region, path-style addressing, chunked encoding and checksum validation. See [Amazon Cloud Storage](getting-started/deployment/storage-backend/amazon-cloud-storage.md).
* Dynamic credential tokens carry a `terrakube_project_name` claim, and federated credentials accept multi-audience OIDC tokens. See [Dynamic Provider Credentials](user-guide/workspaces/dynamic-provider-credentials/README.md).
* Live log viewing scales to many concurrent viewers, and large organizations load faster through aggregated summaries and cursor pagination.
* Spring Boot was upgraded to 4.1.

#### UI

The workspace, run, state, settings and registry pages were refreshed, with a dark theme and accessibility fixes, a responsive layout with a sidebar drawer on narrow screens, and faster initial load.

### August 2026 (2.33.0)

#### Notifications

Organizations and workspaces can now send Slack, Microsoft Teams, or generic webhook notifications when a job's status changes — configurable at either scope, additively. See [Notifications](user-guide/workspaces/notifications.md).

#### Target and Replace Resources in New Run

The native **New Run** dialog gained an **Additional planning options** section for Terraform's `-target`/`-replace` flags, autocompleted against the workspace's actual state resources. See [Starting a Run](user-guide/workspaces/starting-a-run.md).

#### Federated Identity (OIDC) and Variable Collections

Organizations can now trust an external OIDC issuer (e.g. GitHub Actions) to authenticate directly to the API, CLI, and module registry without a stored token — see [Federated Identity](user-guide/organizations/federated-identity.md). Variables can also be grouped into reusable, filterable [Variable Collections](user-guide/organizations/variable-collections.md) shared across workspaces.

#### Structured Job Output, Apply and Imports

The structured plan output view introduced in 2.31.0 now covers `apply` runs and resource imports as well, with real-time diff rendering and strict sensitive-attribute handling. See [Job Output](user-guide/workspaces/job-output.md).

#### Executor Reliability

Executor pools can now safely scale across multiple replicas. Distributed admission control keeps each pod to one job, the dispatcher schedules queued work FIFO and recovers stuck jobs automatically, and a `503` retry backstop handles brief readiness races. Terraform/OpenTofu binaries are cached in cloud storage rather than re-downloaded per job, and Terrakube falls back to the last-known release index if HashiCorp's/OpenTofu's index is briefly unreachable. See [Self-Hosted Agents](getting-started/deployment/self-hosted-agents.md) and [Terraform Versions](user-guide/reference/executor/terraform-versions.md).

#### AWS Dynamic Credentials ABAC

AWS dynamic credentials can now emit session tags in their OIDC JWT. This enables IAM policies based on `aws:PrincipalTag`, so a single role can enforce attribute-based access control across workspaces. See [AWS Dynamic Provider Credentials](user-guide/workspaces/dynamic-provider-credentials/aws-dynamic-provider-credentials.md).

#### Registry and Modules

Module registry versions are now validated and published as canonical SemVer instead of raw git tags, and registered federated credentials now also authenticate registry/module-pull requests.

#### Other improvements

* Workspace and organization list pages were modernized with a condensed, filterable, groupable view (alongside the previous table view).
* Azure DevOps v1/v2 webhooks, GitLab shared repository-level webhooks, and repository auto-discovery make workspace VCS onboarding and webhook management more complete. See [Creating Workspaces](user-guide/workspaces/creating-workspaces.md) and [Azure DevOps](user-guide/vcs-providers/azure-devops.md).
* GitLab joined GitHub for shared-repository webhook consolidation, and approval steps gained an `onReject` command hook.
* An in-app API reference is now available from the **Help & Support** menu.
* Run status and logs now stream via SSE/GraphQL subscriptions instead of polling.

#### Supply-Chain Safeguards

Terrakube's build pipeline now runs OpenSSF Scorecard analysis, generates Syft software bills of materials (SBOMs) for container builds, and uses digest-pinned base images to make build inputs reproducible. These safeguards are built into the release process and require no runtime configuration. See [Security](getting-started/security.md).

### June 2026 (2.32.0)

#### Project Management UI

Projects — introduced in 2.28.0 as an organizational layer above workspaces — gained dedicated management pages in the UI, including project-level team permissions with inline role editing on the project access tab. See [Projects](user-guide/projects/README.md).

#### Terraform Version Constraints

The workspace **Terraform Version** field now accepts semantic version constraints (e.g. `>= 1.12.5`) in addition to an exact version, resolved at run time. See [Terraform Versions](user-guide/reference/executor/terraform-versions.md).

#### VCS Provider Client Editing

VCS provider settings gained an **Edit Client** option for updating OAuth/App client credentials without deleting and recreating the provider connection.

### April 2026 (2.31.0)

#### RBAC v2 Refinements and Structured Plan Output

Following 2.30.0's RBAC v2 rollout, this release introduced the first version of the structured plan output view (expanded further in 2.33.0) and an improved Terraform import experience in the UI.

#### Atlantis-Style PR Workflow

Webhook-triggered jobs gained an Atlantis-style pull request workflow — plan output posted as PR comments, with `terrakube plan`/`terrakube apply` PR comment commands to drive runs.

#### Federated Authentication

Initial federated authentication support landed, handling multiple trusted OIDC issuers, later extended with claim-condition scoping (repository, branch, environment, etc.) and documented in full as [Federated Identity](user-guide/organizations/federated-identity.md) in 2.33.0.

#### Shared Repository Webhooks v3 and Remote State

Shared repository-level VCS webhooks were reworked (v3), and workspaces gained support for global and shared remote state configuration. Executor-to-API communication also moved to JWT-based authentication.

### February 2026 (2.30.0)

#### RBAC v2 and Frontend Redesign

This release shipped RBAC v2 — granular, TFC-style permissions across the organization/project/workspace hierarchy — alongside a broader frontend redesign adopting an HCP Terraform-style UI and performance optimizations. See [Team Management](user-guide/organizations/team-management.md).

#### onFailure Hooks and Job History Retention

Templates gained `onFailure` scripts that run when a Terraform operation fails, and job history retention became configurable per workspace via a `KEEP_JOB_HISTORY` variable.

#### UI Improvements

A new terminal-style job log viewer (with toolbar), sortable workspace and variable listings, sensitive-output copy support, and a modal for editing Variable Collection entries.

### December 2025 (2.29.0)

#### Module and Provider Performance

Module and provider services were refactored onto GraphQL with caching, meaningfully speeding up module/version lookups, alongside job deletion support and importing templates from a private repository.

#### Job and Workspace UI

The job run view now shows workspace and VCS details inline, variable tables gained sortable columns, and dark mode received a round of polish.

### October 2025 (2.28.0)

#### Projects

Terrakube introduced **Projects**, a grouping layer between organizations and workspaces for organizing workspaces by team or environment. See [Projects](user-guide/projects/README.md).

#### Dynamic Credentials and Storage

Dynamic provider credentials gained a configurable hostname override for non-default cloud endpoints, and Cloudflare R2 joined the supported object storage backends alongside AWS, Azure, and GCP.

#### Ephemeral Executor Environment Variables

The `EPHEMERAL_JOB_ENV_VARS` variable was added to pass additional environment variables into ephemeral executor jobs, and bash-script steps gained the ability to persist environment variables for later steps.

### July 2025 (2.27.0)

This release introduces several significant advancements designed to enhance the functionality, security, and integration capabilities of Terrakube.

#### New UI container

The `terrakube-ui` container now utilizes the `nginxinc/nginx-unprivileged` image, replacing the `bitnami/nginx` image used in previous versions.

#### Centralized Terraform Module Version Management

Terraform module versions are now persistently managed within the database, transitioning from the prior Redis-based ephemeral storage. Upon the initial startup following this update, Terrakube will initiate a comprehensive update of all modules. This process may require several minutes, depending on the volume of existing modules.

#### Dynamic Credential Authentication

Enhanced support has been implemented for dynamic credential authentication with Openbao or Vault, streamlining secure access and integration with these systems.

Inside the workspace settings it only requires to use the following variables

<figure><img src=".gitbook/assets/image (1) (1).png" alt=""><figcaption></figcaption></figure>

When running a job for example using the terraform cloud block it will work like the following:

```
terraform plan

Preparing the remote plan...

To view this run in a browser, visit:
https://terrakube-api.platform.local/app/simple/vault/runs/run-1

Waiting for the plan to start...

***************************************
Running Terraform PLAN
***************************************
data.vault_kv_secret_v2.password: Reading...
data.vault_kv_secret_v2.password: Read complete after 0s [id=shared/data/kv/creds]

No changes. Your infrastructure matches the configuration.
```

#### GitLab Integration

Jobs can now be automatically triggered by `merge request` and `release` events within GitLab repositories when defining the webhook configuration like the following:

<figure><img src=".gitbook/assets/image (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

A gitlab webhook will be created inside the repository

<figure><img src=".gitbook/assets/image (2) (1).png" alt=""><figcaption></figcaption></figure>

After every job execution a status will be shown inside gitlab

<figure><img src=".gitbook/assets/image (3) (1).png" alt=""><figcaption></figcaption></figure>

#### Bitbucket Integration

Support has been added for triggering jobs through `pull request` and `release` events in Bitbucket.

<figure><img src=".gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>

#### Improved Development Environment Support

The `devcontainer` environment now offers enhanced support for both PostgreSQL and MSSQL databases, facilitating more flexible development workflows.

<figure><img src=".gitbook/assets/image (6).png" alt=""><figcaption></figcaption></figure>

#### Internal System Migration

The entire project codebase has been migrated to the `io.terrakube` package structure. Concurrently, all previously defined Quartz jobs have been successfully migrated to this new package hierarchy, ensuring consistency and maintainability.

Older releases (2.11 to 2.22) are in the [archive](updates-archive.md).
