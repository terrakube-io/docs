# `terrakube workspace-schedule update`

Updates cron expression or template reference for a workspace schedule.

## Usage

```bash
terrakube workspace-schedule update --organization <ORG_NAME_OR_ID> --workspace <WORKSPACE_NAME_OR_ID> --id <SCHEDULE_ID> [flags]
```

## Flags

| Flag | Short | Type | Required | Description |
| :--- | :--- | :--- | :--- | :--- |
| `--organization` | `-o` | String | Yes | Name or ID of the parent organization. |
| `--workspace` | `-w` | String | Yes | Name or ID of the parent workspace. |
| `--id` | | String | Yes | UUID of the schedule. |
| `--schedule` | | String | No | Updated cron expression. |
| `--template-id` | | String | No | Updated template ID. |
| `--output` | | String | No | Output format (`json`, `table`, `yaml`, `tsv`). |

## Examples

### Update schedule to run weekly
```bash
terrakube workspace-schedule update \
  -o "demo-org" \
  -w "docker-compose-infra" \
  --id "123e4567-e89b-12d3-a456-426614174000" \
  --schedule "0 0 * * 0" \
  --output table
```
