# Terrakube CLI Commands Reference

This section provides complete documentation and usage examples for all resource subcommands in `terrakube`.

## Available Command Domains

| Command Domain | Description |
| :--- | :--- |
| **[Authentication](auth/README.md)** | Authenticate (`login`) and end sessions (`logout`). |
| **[Organization](organization/README.md)** | Manage Terrakube organizations. |
| **[Team](team/README.md)** | Manage teams and organization role permissions. |
| **[Workspace](workspace/README.md)** | Manage workspaces, VCS sources, OpenTofu/Terraform execution settings. |
| **[Workspace Variable](variable/README.md)** | Manage environment (ENV) and Terraform (TERRAFORM) workspace variables. |
| **[Workspace Tag](workspace-tag/README.md)** | Associate tags with workspaces. |
| **[Provider](provider/README.md)** | Manage private registry providers. |
| **[Provider Version](provider-version/README.md)** | Manage private provider release versions and protocols. |
| **[Provider Implementation](implementation/README.md)** | Manage architecture binaries, SHA256 checksums, and PGP signatures for providers. |
| **[Organization Module](module/README.md)** | Manage private registry Terraform modules. |
| **[Organization Variable](organization-variable/README.md)** | Manage global variables shared across all organization workspaces. |
| **[Organization Agent](agent/README.md)** | Register and manage self-hosted agent execution endpoints. |
| **[SSH Key](ssh/README.md)** | Store SSH private keys for private VCS git repositories. |
| **[Workspace Access](workspace-access/README.md)** | Configure team permissions on specific workspaces. |
| **[Template](template/README.md)** | Define reusable Terrakube TCL workflow flow templates. |
| **[Collection](collection/README.md)** | Create reusable variable collections. |
| **[Collection Item](collection-item/README.md)** | Add key/value items to collections. |
| **[Collection Reference](collection-reference/README.md)** | Link collections to target workspaces. |
| **[Project](project/README.md)** | Group workspaces into logical projects. |
| **[Project Access](project-access/README.md)** | Set team access permissions on projects. |
| **[Federated Identity](federated/README.md)** | Configure OIDC federated identity providers. |
| **[Federated Claim](federated-claim/README.md)** | Define federated identity claims for workload identity authentication. |
