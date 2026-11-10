# `terrakube notification-trigger get`

Retrieves details of a specific notification trigger.

## Usage

```bash
terrakube notification-trigger get --organization <ORG_NAME_OR_ID> --notification-configuration <CONFIG_NAME_OR_ID> --id <TRIGGER_ID> [flags]
```

## Flags

| Flag | Short | Type | Required | Description |
| :--- | :--- | :--- | :--- | :--- |
| `--organization` | `-o` | String | Yes | Name or ID of the parent organization. |
| `--notification-configuration` | `-n` | String | Yes | Name or ID of the parent notification configuration. |
| `--id` | | String | Yes | UUID of the notification trigger. |
| `--output` | | String | No | Output format (`json`, `table`, `yaml`, `tsv`). |

## Examples

### Get trigger details
```bash
terrakube notification-trigger get -o "demo-org" -n "slack-alerts" --id "12345678-1234-1234-1234-123456789abc" --output json
```
