# `terrakube workspace-schedule create`

Creates a new automated cron schedule for a workspace.

## Usage

```bash
terrakube workspace-schedule create --organization <ORG_NAME_OR_ID> --workspace <WORKSPACE_NAME_OR_ID> --schedule <CRON> --template-id <TEMPLATE_ID> [flags]
```

## Flags

| Flag | Short | Type | Required | Description |
| :--- | :--- | :--- | :--- | :--- |
| `--organization` | `-o` | String | Yes | Name or ID of the parent organization. |
| `--workspace` | `-w` | String | Yes | Name or ID of the parent workspace. |
| `--schedule` | | String | Yes | Standard 5/6-field cron expression (e.g. `0 0 * * *`). |
| `--template-id` | | String | Yes | Template reference UUID. |
| `--output` | | String | No | Output format (`json`, `table`, `yaml`, `tsv`). |

## Examples

### Create a nightly drift detection schedule
```bash
terrakube workspace-schedule create \
  -o "demo-org" \
  -w "docker-compose-infra" \
  --schedule "0 2 * * *" \
  --template-id "123e4567-e89b-12d3-a456-426614174000" \
  --output json
```
