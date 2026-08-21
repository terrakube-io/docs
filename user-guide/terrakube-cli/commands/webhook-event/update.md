# `terrakube webhook-event update`

Updates attributes of an existing webhook event trigger rule.

## Usage

```bash
terrakube webhook-event update --organization <ORG_NAME_OR_ID> --workspace <WORKSPACE_NAME_OR_ID> --webhook-id <WEBHOOK_ID> --id <EVENT_ID> [flags]
```

## Flags

| Flag | Short | Type | Required | Description |
| :--- | :--- | :--- | :--- | :--- |
| `--organization` | `-o` | String | Yes | Name or ID of the parent organization. |
| `--workspace` | `-w` | String | Yes | Name or ID of the parent workspace. |
| `--webhook-id` | | String | Yes | ID of the parent webhook. |
| `--id` | | String | Yes | UUID of the webhook event. |
| `--branch` | | String | No | Updated branch pattern. |
| `--event` | | String | No | Updated event type. |
| `--path` | | String | No | Updated path. |
| `--priority` | | Int | No | Updated priority. |
| `--output` | | String | No | Output format (`json`, `table`, `yaml`, `tsv`). |

## Examples

### Update webhook event branch
```bash
terrakube webhook-event update \
  -o "demo-org" \
  -w "docker-compose-infra" \
  --webhook-id "123e4567-e89b-12d3-a456-426614174000" \
  --id "890e1234-e89b-12d3-a456-426614174000" \
  --branch "release/*" \
  --output table
```
