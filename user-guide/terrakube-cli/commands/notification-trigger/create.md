# `terrakube notification-trigger create`

Creates a new trigger condition on a notification configuration.

## Usage

```bash
terrakube notification-trigger create --organization <ORG_NAME_OR_ID> --notification-configuration <CONFIG_NAME_OR_ID> --job-status <STATUS> [flags]
```

## Flags

| Flag | Short | Type | Required | Description |
| :--- | :--- | :--- | :--- | :--- |
| `--organization` | `-o` | String | Yes | Name or ID of the parent organization. |
| `--notification-configuration` | `-n` | String | Yes | Name or ID of the parent notification configuration. |
| `--job-status` | `-s` | String | Yes | Job status triggering notification (`completed`, `failed`, `running`, `cancelled`, `pendingApproval`). |
| `--output` | | String | No | Output format (`json`, `table`, `yaml`, `tsv`). |

## Examples

### Create a trigger on job completion
```bash
terrakube notification-trigger create \
  -o "demo-org" \
  -n "slack-alerts" \
  --job-status "completed" \
  --output table
```

### Create a trigger on job failure
```bash
terrakube notification-trigger create \
  -o "demo-org" \
  -n "slack-alerts" \
  --job-status "failed" \
  --output json
```
