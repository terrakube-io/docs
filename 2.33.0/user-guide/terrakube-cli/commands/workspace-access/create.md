# `terrakube workspace-access create`

Grants team permissions on a workspace.

## Usage

```bash
terrakube workspace-access create --organization <ORG> --workspace <WS> --name <TEAM_NAME> [flags]
```

## Flags

| Flag | Short | Type | Required | Description |
| :--- | :--- | :--- | :--- | :--- |
| `--organization` | `-o` | String | Yes | Organization ID or name. |
| `--workspace` | `-w` | String | Yes | Workspace ID or name. |
| `--name` | `-n` | String | Yes | Team name (e.g. `TERRAKUBE_ADMIN`). |
| `--manage-state` | | Bool | No | Permission to manage state files. |
| `--manage-workspace` | | Bool | No | Permission to manage workspace settings. |
| `--manage-job` | | Bool | No | Permission to trigger jobs. |

## Examples

```bash
terrakube workspace-access create -o "$ORG_ID" -w "$WS_ID" \
  --name "TERRAKUBE_ADMIN" \
  --manage-state \
  --manage-workspace \
  --manage-job \
  --output json
```
