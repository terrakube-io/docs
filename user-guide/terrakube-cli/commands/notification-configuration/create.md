# `terrakube notification-configuration create`

Creates a new notification configuration for Slack, Microsoft Teams, or custom Webhook integrations.

## Usage

```bash
terrakube notification-configuration create --organization <ORG_NAME_OR_ID> --name <NAME> --channel-type <TYPE> --destination-url <URL> [flags]
```

## Flags

| Flag | Short | Type | Required | Description |
| :--- | :--- | :--- | :--- | :--- |
| `--organization` | `-o` | String | Yes | Name or ID of the parent organization. |
| `--workspace` | `-w` | String | No | Name or ID of the workspace (to scope configuration to a specific workspace). |
| `--name` | `-n` | String | Yes | Notification configuration name. |
| `--description` | `-d` | String | No | Notification configuration description. |
| `--channel-type` | | String | Yes | Channel type (`SLACK`, `TEAMS`, `WEBHOOK`). |
| `--destination-url` | | String | Yes | Destination webhook URL endpoint. |
| `--signing-secret` | | String | No | Signing secret for webhook payload verification (HMAC). |
| `--active` | | Bool | No | Whether the notification configuration is active. |
| `--message-style` | | String | No | Message presentation style (`DETAILED`, `SIMPLE`). |
| `--output` | | String | No | Output format (`json`, `table`, `yaml`, `tsv`). |

## Examples

### Create a Slack notification configuration
```bash
terrakube notification-configuration create \
  -o "demo-org" \
  --name "slack-alerts" \
  --description "Production alerts channel" \
  --channel-type "SLACK" \
  --destination-url "https://hooks.slack.com/services/T00/B00/X00" \
  --message-style "DETAILED" \
  --active \
  --output json
```

### Create a workspace-scoped Microsoft Teams notification configuration
```bash
terrakube notification-configuration create \
  -o "demo-org" \
  -w "infra-workspace" \
  --name "teams-deployment-channel" \
  --channel-type "TEAMS" \
  --destination-url "https://outlook.office.com/webhook/..." \
  --message-style "SIMPLE" \
  --active \
  --output table
```
