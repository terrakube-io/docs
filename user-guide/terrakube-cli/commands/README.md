# Terrakube CLI Commands Reference

This section provides complete documentation and usage examples for all resource subcommands in `terrakube`.

## Available Command Domains

| Command Domain | Description |
| :--- | :--- |
| **[Authentication](auth/README.md)** | Authenticate (`login`) and end sessions (`logout`). |
| **[Organization](organization/README.md)** | Manage Terrakube organizations. |
| **[Organization Tag](tag/README.md)** | Create and list organization tags. |
| **[Team](team/README.md)** | Manage teams and organization role permissions. |
| **[Team Token](team-token/README.md)** | Generate, list, and delete team API access tokens. |
| **[Workspace](workspace/README.md)** | Manage workspaces, VCS sources, OpenTofu/Terraform execution settings. |
| **[Workspace Variable](variable/README.md)** | Manage environment (ENV) and Terraform (TERRAFORM) workspace variables. |
| **[Workspace Tag](workspace-tag/README.md)** | Associate tags with workspaces. |
| **[Workspace Schedule](workspace-schedule/README.md)** | Configure automated cron schedules and template triggers for workspaces. |
| **[Workspace Access](workspace-access/README.md)** | Configure team permissions on specific workspaces. |
| **[Workspace Action](action/README.md)** | Manage custom UI action buttons and execution criteria. |
| **[Workspace Address](address/README.md)** | Manage workspace remote state address mappings. |
| **[Notification Configuration](notification-configuration/README.md)** | Manage Slack, Microsoft Teams, and custom Webhook notification integrations. |
| **[Notification Trigger](notification-trigger/README.md)** | Define status triggers (`completed`, `failed`, etc.) for notification configurations. |
| **[Job](job/README.md)** | Trigger, inspect, and cancel Terraform/OpenTofu jobs (`plan`, `apply`, `destroy`). |
| **[Job Step](step/README.md)** | Inspect and update individual workflow execution steps. |
| **[Job History](history/README.md)** | View workspace execution history and state serial revisions. |
| **[Operations](operations/README.md)** | Execute atomic JSON:API batch operation files (`atomic:operations`). |
| **[VCS](vcs/README.md)** | Connect and configure VCS providers (GitHub, GitLab, Bitbucket, Azure DevOps). |
| **[Webhook](webhook/README.md)** | Configure workspace VCS webhooks for automated triggering. |
| **[Webhook Event](webhook-event/README.md)** | Map branch patterns and event types to templates. |
| **[GitHub App Token](github-app-token/README.md)** | Manage GitHub App installation tokens. |
| **[Provider](provider/README.md)** | Manage private registry providers. |
| **[Provider Version](provider-version/README.md)** | Manage private provider release versions and protocols. |
| **[Provider Implementation](implementation/README.md)** | Manage architecture binaries, SHA256 checksums, and PGP signatures for providers. |
| **[Organization Module](module/README.md)** | Manage private registry Terraform modules. |
| **[Module Version](module-version/README.md)** | Manage release versions and git references for private modules. |
| **[Organization Variable](organization-variable/README.md)** | Manage global variables shared across all organization workspaces. |
| **[Organization Agent](agent/README.md)** | Register and manage self-hosted agent execution endpoints. |
| **[SSH Key](ssh/README.md)** | Store SSH private keys for private VCS git repositories. |
| **[Template](template/README.md)** | Define reusable Terrakube TCL workflow flow templates. |
| **[Collection](collection/README.md)** | Create reusable variable collections. |
| **[Collection Item](collection-item/README.md)** | Add key/value items to collections. |
| **[Collection Reference](collection-reference/README.md)** | Link collections to target workspaces. |
| **[Project](project/README.md)** | Group workspaces into logical projects. |
| **[Project Access](project-access/README.md)** | Set team access permissions on projects. |
| **[Federated Identity](federated/README.md)** | Configure OIDC federated identity providers. |
| **[Federated Claim](federated-claim/README.md)** | Define federated identity claims for workload identity authentication. |

