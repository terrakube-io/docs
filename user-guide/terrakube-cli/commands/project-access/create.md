# `terrakube project-access create`

Grants a team access permissions to a project.

## Usage

```bash
terrakube project-access create --organization <ORG> --project <PRJ> --name <TEAM_NAME> [flags]
```

## Flags

| Flag | Short | Type | Required | Description |
| :--- | :--- | :--- | :--- | :--- |
| `--organization` | `-o` | String | Yes | Organization ID or name. |
| `--project` | | String | Yes | Project ID or name. |
| `--name` | `-n` | String | Yes | Team name (e.g. `TERRAKUBE_ADMIN`). |
| `--manage-state` | | Bool | No | Grant state management. |
| `--manage-workspace` | | Bool | No | Grant workspace management. |
| `--manage-job` | | Bool | No | Grant job management. |

## Examples

```bash
terrakube project-access create -o "$ORG_ID" \
  --project "$PRJ_ID" \
  --name "TERRAKUBE_ADMIN" \
  --manage-state \
  --manage-workspace \
  --manage-job \
  --output json
```
