# Introduction

Terrakube is an open source platform for running Terraform and OpenTofu remotely, with the collaboration, governance and private registry you would otherwise get from Terraform Enterprise, Scalr or env0. It runs on any Kubernetes cluster (or Docker Compose for evaluation) and works with the tools you already use.

### What you get

* **Workspaces and runs**: plans and applies run remotely from a git repository, the Terraform CLI, or the API, with approvals, scheduling, drift detection and structured, live output.
* **Governance**: organizations, projects, teams with role-based access, [OPA policy enforcement](user-guide/policy-enforcement-opa.md), cost estimation and audit trails.
* **Private registry**: host your own Terraform modules and providers, or mirror the public registry.
* **Integrations**: GitHub, GitLab, Bitbucket and Azure DevOps, Slack and Teams notifications, OIDC dynamic credentials for AWS, Azure, GCP and Vault, and a CLI, API and Terraform provider to automate Terrakube itself.
* **Extensible**: custom workflows in [Terrakube Configuration Language](user-guide/organizations/templates/README.md) and actions that add buttons to the workspace UI.

### Where to start

| I want to... | Go to |
| --- | --- |
| Try Terrakube on my machine | [Getting Started](getting-started/getting-started.md) |
| Deploy it for my team | [Deployment](getting-started/deployment/README.md) and [User Management](getting-started/user-management/README.md) |
| Run my first workspace | [Organizations](user-guide/organizations/README.md), then [Workspaces](user-guide/workspaces/README.md) |
| Connect a git provider | [VCS Providers](user-guide/vcs-providers/README.md) |
| Use it from the Terraform CLI | [CLI-driven Workflow](user-guide/workspaces/cli-driven-workflow.md) |
| Run it from CI/CD | [CI/CD Integration](user-guide/ci-cd-integration/README.md) |
| Automate Terrakube itself | [Terrakube CLI](user-guide/terrakube-cli/README.md), [API](api/getting-started.md) and the [Terraform provider](https://registry.terraform.io/providers/terrakube-io/terrakube/latest/docs) |
| Move from Terraform Cloud/Enterprise | [Migrating to Terrakube](user-guide/migrating-to-terrakube.md) |
| See what changed | [Updates](updates.md) |

### Sponsors

| Company | Service |
| --- | --- |
| <p> </p><div><figure><img src=".gitbook/assets/jetbrains.png" alt="JetBrains" width="188"><figcaption></figcaption></figure></div><p><a href="https://jb.gg/OpenSource">JetBrains</a></p> | For providing with free licenses to their great tools. |
| <p></p><div><figure><img src=".gitbook/assets/image (482).png" alt="GitBook" width="128"><figcaption></figcaption></figure></div><p><a href="https://www.gitbook.com/">GitBook</a></p> | For providing us with free OSS Plan. |
| <div><figure><img src=".gitbook/assets/image (7).png" alt="Docker" width="128"><figcaption></figcaption></figure></div><p><a href="https://www.docker.com/">Docker</a></p> | For providing us with free OSS Plan. |
| <p></p><div><figure><img src=".gitbook/assets/tuta_logo.svg" alt="Tuta" width="188"><figcaption></figcaption></figure></div><p><a href="https://tuta.com/">Tuta</a></p> | For providing us with free email service. |
