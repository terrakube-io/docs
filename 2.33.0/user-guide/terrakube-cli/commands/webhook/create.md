# `terrakube webhook create`

Creates a new VCS webhook for a workspace.

## Usage

```bash
terrakube webhook create --organization <ORG_NAME_OR_ID> --workspace <WORKSPACE_NAME_OR_ID> [flags]
```

## Flags

| Flag | Short | Type | Required | Description |
| :--- | :--- | :--- | :--- | :--- |
| `--organization` | `-o` | String | Yes | Name or ID of the parent organization. |
| `--workspace` | `-w` | String | Yes | Name or ID of the parent workspace. |
| `--branch` | | String | No | Git branch to watch. |
| `--path` | | String | No | Webhook path. |
| `--template-id` | | String | No | Workflow template ID to trigger. |
| `--remote-hook-id` | | String | No | VCS remote hook identifier. |
| `--event` | | String | No | Event type to trigger on. |
| `--output` | | String | No | Output format (`json`, `table`, `yaml`, `tsv`). |

## Examples

### Create a workspace webhook
```bash
terrakube webhook create \
  -o "demo-org" \
  -w "docker-compose-infra" \
  --branch "main" \
  --output json
```
