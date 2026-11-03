# Introduction

Terrakube is an open source platform for running Terraform and OpenTofu remotely, with the collaboration, governance and private registry you would otherwise get from Terraform Enterprise, Scalr or env0. It runs on any Kubernetes cluster (or Docker Compose for evaluation) and works with the tools you already use.

### What you get

* **Workspaces and runs**: plans and applies run remotely from a git repository, the Terraform CLI, or the API, with approvals, scheduling, drift detection and structured, live output.
* **Governance**: organizations, projects, teams with role-based access, [OPA policy enforcement](govern/policy-enforcement-opa/), cost estimation and audit trails.
* **Private registry**: host your own Terraform modules and providers, or mirror the public registry.
* **Integrations**: GitHub, GitLab, Bitbucket and Azure DevOps, Slack and Teams notifications, OIDC dynamic credentials for AWS, Azure, GCP and Vault, and a CLI, API and Terraform provider to automate Terrakube itself.
* **Extensible**: custom workflows in [Terrakube Configuration Language](set-up/organizations/templates/) and actions that add buttons to the workspace UI.

### Where to start

| I want to...                              | Go to                                                                                                                                                                                       |
| ----------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Try Terrakube on my machine               | [Getting Started](getting-started/getting-started.md)                                                                                                                                       |
| Deploy it for my team                     | [Deployment](getting-started/deployment/) and [User Management](getting-started/user-management/)                                                                                           |
| Run my first workspace                    | [Organizations](set-up/organizations/), then [Workspaces](run-terraform/workspaces/)                                                                                                        |
| Connect a git provider                    | [VCS Providers](set-up/vcs-providers/)                                                                                                                                                      |
| Use it from the Terraform CLI             | [CLI-driven Workflow](run-terraform/workspaces/cli-driven-workflow.md)                                                                                                                      |
| Run it from CI/CD                         | [CI/CD Integration](integrate/ci-cd-integration/)                                                                                                                                           |
| Automate Terrakube itself                 | [Terrakube CLI](integrate/terrakube-cli/), [API](api-reference/getting-started.md) and the [Terraform provider](https://registry.terraform.io/providers/terrakube-io/terrakube/latest/docs) |
| Move from Terraform Cloud/Enterprise      | [Migrating to Terrakube](reference/migrating-to-terrakube.md)                                                                                                                               |
| Look up a setting or environment variable | [Configuration Reference](reference/configuration/)                                                                                                                                         |
| Fix a problem                             | [Troubleshooting](reference/troubleshooting.md)                                                                                                                                             |
| Upgrade                                   | [Upgrading](getting-started/upgrading.md)                                                                                                                                                   |
| See what changed                          | [Updates](updates/)                                                                                                                                                                         |

### Sponsors

| Company                                                                                                                             | Service                                                |
| ----------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------ |
| <p><img src=".gitbook/assets/jetbrains.png" alt="" data-size="original"></p><p><a href="https://jb.gg/OpenSource">JetBrains</a></p> | For providing with free licenses to their great tools. |
| <p><img src=".gitbook/assets/image (482).png" alt="" data-size="original"></p><p><a href="https://www.gitbook.com/">GitBook</a></p> | For providing us with free OSS Plan.                   |
| <p><img src=".gitbook/assets/image (7).png" alt="" data-size="original"></p><p><a href="https://www.docker.com/">Docker</a></p>     | For providing us with free OSS Plan.                   |
| <p><img src=".gitbook/assets/tuta_logo.svg" alt="" data-size="original"></p><p><a href="https://tuta.com/">Tuta</a></p>             | For providing us with free email service.              |
