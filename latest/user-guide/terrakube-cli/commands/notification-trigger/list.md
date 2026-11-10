# `terrakube notification-trigger list`

Lists triggers associated with a notification configuration.

## Usage

```bash
terrakube notification-trigger list --organization <ORG_NAME_OR_ID> --notification-configuration <CONFIG_NAME_OR_ID> [flags]
```

## Flags

| Flag | Short | Type | Required | Description |
| :--- | :--- | :--- | :--- | :--- |
| `--organization` | `-o` | String | Yes | Name or ID of the parent organization. |
| `--notification-configuration` | `-n` | String | Yes | Name or ID of the parent notification configuration. |
| `--output` | | String | No | Output format (`json`, `table`, `yaml`, `tsv`). |

## Examples

### List all triggers for a notification configuration
```bash
terrakube notification-trigger list -o "demo-org" -n "slack-alerts" --output table
```
