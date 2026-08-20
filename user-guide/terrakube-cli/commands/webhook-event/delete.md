# `terrakube webhook-event delete`

Deletes a webhook event rule.

## Usage

```bash
terrakube webhook-event delete --organization <ORG_NAME_OR_ID> --workspace <WORKSPACE_NAME_OR_ID> --webhook-id <WEBHOOK_ID> --id <EVENT_ID> [flags]
```

## Flags

| Flag | Short | Type | Required | Description |
| :--- | :--- | :--- | :--- | :--- |
| `--organization` | `-o` | String | Yes | Name or ID of the parent organization. |
| `--workspace` | `-w` | String | Yes | Name or ID of the parent workspace. |
| `--webhook-id` | | String | Yes | ID of the parent webhook. |
| `--id` | | String | Yes | UUID of the webhook event to delete. |

## Examples

### Delete a webhook event rule
```bash
terrakube webhook-event delete \
  -o "demo-org" \
  -w "docker-compose-infra" \
  --webhook-id "123e4567-e89b-12d3-a456-426614174000" \
  --id "890e1234-e89b-12d3-a456-426614174000"
```
