# `terrakube webhook-event list`

Lists all event rules configured for a webhook.

## Usage

```bash
terrakube webhook-event list --organization <ORG_NAME_OR_ID> --workspace <WORKSPACE_NAME_OR_ID> --webhook-id <WEBHOOK_ID> [flags]
```

## Flags

| Flag | Short | Type | Required | Description |
| :--- | :--- | :--- | :--- | :--- |
| `--organization` | `-o` | String | Yes | Name or ID of the parent organization. |
| `--workspace` | `-w` | String | Yes | Name or ID of the parent workspace. |
| `--webhook-id` | | String | Yes | ID of the parent webhook. |
| `--output` | | String | No | Output format (`json`, `table`, `yaml`, `tsv`). |

## Examples

### List webhook event rules
```bash
terrakube webhook-event list -o "demo-org" -w "docker-compose-infra" --webhook-id "123e4567-e89b-12d3-a456-426614174000" --output table
```
