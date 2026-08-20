# `terrakube notification-configuration delete`

Deletes a notification configuration.

## Usage

```bash
terrakube notification-configuration delete --organization <ORG_NAME_OR_ID> --id <CONFIG_ID> [flags]
```

## Flags

| Flag | Short | Type | Required | Description |
| :--- | :--- | :--- | :--- | :--- |
| `--organization` | `-o` | String | Yes | Name or ID of the parent organization. |
| `--id` | | String | Yes | UUID of the notification configuration. |

## Examples

### Delete a notification configuration
```bash
terrakube notification-configuration delete -o "demo-org" --id "270db279-b1d5-4fd3-ab69-2f58e458e0a1"
```
