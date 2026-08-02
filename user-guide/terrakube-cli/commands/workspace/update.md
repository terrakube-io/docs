# `terrakube workspace update`

Updates workspace configuration parameters, project assignments, or soft-deletes a workspace.

## Usage

```bash
terrakube workspace update --organization <ORG> --id <WS_ID> [flags]
```

## Flags

| Flag | Short | Type | Required | Description |
| :--- | :--- | :--- | :--- | :--- |
| `--organization` | `-o` | String | Yes | Organization ID or name. |
| `--id` | | String | Yes | Workspace ID. |
| `--name` | `-n` | String | No | Workspace name. |
| `--description` | `-d` | String | No | Description. |
| `--project` | | String | No | Project ID or name to assign workspace to. |
| `--deleted` | | Bool | No | Soft-delete flag (marks workspace as deleted). |
| `--source` | | String | No | VCS repository URL. |
| `--branch` | | String | No | Repository branch. |
| `--folder` | | String | No | Repository folder path. |
| `--iac-type` | | String | No | Tool type (`tofu`, `terraform`). |
| `--iac-version` | | String | No | Tool version. |

## Examples

### Assign workspace to a project
```bash
terrakube workspace update -o "$ORG_ID" --id "$WS_ID" --name "networking-prod" --project "$PRJ_ID"
```

### Soft-delete a workspace
```bash
terrakube workspace update -o "$ORG_ID" --id "$WS_ID" --name "temp-ws" --deleted
```
