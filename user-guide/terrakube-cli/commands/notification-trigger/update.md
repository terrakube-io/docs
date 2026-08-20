# `terrakube notification-trigger update`

Updates an existing notification trigger.

## Usage

```bash
terrakube notification-trigger update --organization <ORG_NAME_OR_ID> --notification-configuration <CONFIG_NAME_OR_ID> --id <TRIGGER_ID> [flags]
```

## Flags

| Flag | Short | Type | Required | Description |
| :--- | :--- | :--- | :--- | :--- |
| `--organization` | `-o` | String | Yes | Name or ID of the parent organization. |
| `--notification-configuration` | `-n` | String | Yes | Name or ID of the parent notification configuration. |
| `--id` | | String | Yes | UUID of the notification trigger. |
| `--job-status` | `-s` | String | No | Updated job status condition. |
| `--output` | | String | No | Output format (`json`, `table`, `yaml`, `tsv`). |

## Examples

### Update trigger status condition
```bash
terrakube notification-trigger update \
  -o "demo-org" \
  -n "slack-alerts" \
  --id "12345678-1234-1234-1234-123456789abc" \
  --job-status "cancelled" \
  --output json
```
