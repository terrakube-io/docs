# `terrakube webhook delete`

Deletes a workspace webhook.

## Usage

```bash
terrakube webhook delete --organization <ORG_NAME_OR_ID> --workspace <WORKSPACE_NAME_OR_ID> --id <WEBHOOK_ID> [flags]
```

## Flags

| Flag | Short | Type | Required | Description |
| :--- | :--- | :--- | :--- | :--- |
| `--organization` | `-o` | String | Yes | Name or ID of the parent organization. |
| `--workspace` | `-w` | String | Yes | Name or ID of the parent workspace. |
| `--id` | | String | Yes | UUID of the webhook to delete. |

## Examples

### Delete a webhook
```bash
terrakube webhook delete -o "demo-org" -w "docker-compose-infra" --id "123e4567-e89b-12d3-a456-426614174000"
```
