# `terrakube notification-configuration update`

Updates an existing notification configuration.

## Usage

```bash
terrakube notification-configuration update --organization <ORG_NAME_OR_ID> --id <CONFIG_ID> [flags]
```

## Flags

| Flag | Short | Type | Required | Description |
| :--- | :--- | :--- | :--- | :--- |
| `--organization` | `-o` | String | Yes | Name or ID of the parent organization. |
| `--id` | | String | Yes | UUID of the notification configuration. |
| `--name` | `-n` | String | No | Updated name. |
| `--description` | `-d` | String | No | Updated description. |
| `--channel-type` | | String | No | Channel type (`SLACK`, `TEAMS`, `WEBHOOK`). |
| `--destination-url` | | String | No | Destination URL. |
| `--signing-secret` | | String | No | Signing secret. |
| `--active` | | Bool | No | Set active/inactive status. |
| `--message-style` | | String | No | Message presentation style (`DETAILED`, `SIMPLE`). |
| `--output` | | String | No | Output format (`json`, `table`, `yaml`, `tsv`). |

## Examples

### Update destination URL and message style
```bash
terrakube notification-configuration update \
  -o "demo-org" \
  --id "270db279-b1d5-4fd3-ab69-2f58e458e0a1" \
  --destination-url "https://hooks.slack.com/services/NEW/URL" \
  --message-style "SIMPLE" \
  --output json
```
