# `terrakube notification-trigger delete`

Deletes a notification trigger.

## Usage

```bash
terrakube notification-trigger delete --organization <ORG_NAME_OR_ID> --notification-configuration <CONFIG_NAME_OR_ID> --id <TRIGGER_ID> [flags]
```

## Flags

| Flag | Short | Type | Required | Description |
| :--- | :--- | :--- | :--- | :--- |
| `--organization` | `-o` | String | Yes | Name or ID of the parent organization. |
| `--notification-configuration` | `-n` | String | Yes | Name or ID of the parent notification configuration. |
| `--id` | | String | Yes | UUID of the notification trigger to delete. |

## Examples

### Delete a trigger
```bash
terrakube notification-trigger delete \
  -o "demo-org" \
  -n "slack-alerts" \
  --id "12345678-1234-1234-1234-123456789abc"
```
