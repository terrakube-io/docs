# Commands

This section provides complete documentation and usage examples for all resource subcommands in `terrakube`.

## Available Command Domains

| Command Domain                                                | Description                                                                           |
| ------------------------------------------------------------- | ------------------------------------------------------------------------------------- |
| [**Authentication**](auth/)                                   | Authenticate (`login`) and end sessions (`logout`).                                   |
| [**Organization**](organization/)                             | Manage Terrakube organizations.                                                       |
| [**Organization Tag**](tag/)                                  | Create and list organization tags.                                                    |
| [**Team**](team/)                                             | Manage teams and organization role permissions.                                       |
| [**Team Token**](team-token/)                                 | Generate, list, and delete team API access tokens.                                    |
| [**Workspace**](workspace/)                                   | Manage workspaces, VCS sources, OpenTofu/Terraform execution settings.                |
| [**Workspace Variable**](variable/)                           | Manage environment (ENV) and Terraform (TERRAFORM) workspace variables.               |
| [**Workspace Tag**](workspace-tag/)                           | Associate tags with workspaces.                                                       |
| [**Workspace Schedule**](workspace-schedule/)                 | Configure automated cron schedules and template triggers for workspaces.              |
| [**Workspace Access**](workspace-access/)                     | Configure team permissions on specific workspaces.                                    |
| [**Workspace Action**](action/)                               | Manage custom UI action buttons and execution criteria.                               |
| [**Workspace Address**](address/)                             | Manage workspace remote state address mappings.                                       |
| [**Notification Configuration**](notification-configuration/) | Manage Slack, Microsoft Teams, and custom Webhook notification integrations.          |
| [**Notification Trigger**](notification-trigger/)             | Define status triggers (`completed`, `failed`, etc.) for notification configurations. |
| [**Job**](job/)                                               | Trigger, inspect, and cancel Terraform/OpenTofu jobs (`plan`, `apply`, `destroy`).    |
| [**Job Step**](step/)                                         | Inspect and update individual workflow execution steps.                               |
| [**Job History**](history/)                                   | View workspace execution history and state serial revisions.                          |
| [**Operations**](operations.md)                               | Execute atomic JSON:API batch operation files (`atomic:operations`).                  |
| [**VCS**](vcs/)                                               | Connect and configure VCS providers (GitHub, GitLab, Bitbucket, Azure DevOps).        |
| [**Webhook**](webhook/)                                       | Configure workspace VCS webhooks for automated triggering.                            |
| [**Webhook Event**](webhook-event/)                           | Map branch patterns and event types to templates.                                     |
| [**GitHub App Token**](github-app-token/)                     | Manage GitHub App installation tokens.                                                |
| [**Provider**](provider/)                                     | Manage private registry providers.                                                    |
| [**Provider Version**](provider-version/)                     | Manage private provider release versions and protocols.                               |
| [**Provider Implementation**](implementation/)                | Manage architecture binaries, SHA256 checksums, and PGP signatures for providers.     |
| [**Organization Module**](module/)                            | Manage private registry Terraform modules.                                            |
| [**Module Version**](module-version/)                         | Manage release versions and git references for private modules.                       |
| [**Organization Variable**](organization-variable/)           | Manage global variables shared across all organization workspaces.                    |
| [**Organization Agent**](agent/)                              | Register and manage self-hosted agent execution endpoints.                            |
| [**SSH Key**](ssh/)                                           | Store SSH private keys for private VCS git repositories.                              |
| [**Template**](template/)                                     | Define reusable Terrakube TCL workflow flow templates.                                |
| [**Collection**](collection/)                                 | Create reusable variable collections.                                                 |
| [**Collection Item**](collection-item/)                       | Add key/value items to collections.                                                   |
| [**Collection Reference**](collection-reference/)             | Link collections to target workspaces.                                                |
| [**Project**](project/)                                       | Group workspaces into logical projects.                                               |
| [**Project Access**](project-access/)                         | Set team access permissions on projects.                                              |
| [**Federated Identity**](federated/)                          | Configure OIDC federated identity providers.                                          |
| [**Federated Claim**](federated-claim/)                       | Define federated identity claims for workload identity authentication.                |
