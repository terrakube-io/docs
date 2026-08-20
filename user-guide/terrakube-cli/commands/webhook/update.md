# `terrakube webhook update`

Updates attributes of an existing workspace webhook.

## Usage

```bash
terrakube webhook update --organization <ORG_NAME_OR_ID> --workspace <WORKSPACE_NAME_OR_ID> --id <WEBHOOK_ID> [flags]
```

## Flags

| Flag | Short | Type | Required | Description |
| :--- | :--- | :--- | :--- | :--- |
| `--organization` | `-o` | String | Yes | Name or ID of the parent organization. |
| `--workspace` | `-w` | String | Yes | Name or ID of the parent workspace. |
| `--id` | | String | Yes | UUID of the webhook. |
| `--branch` | | String | No | Updated branch name. |
| `--path` | | String | No | Updated path. |
| `--template-id` | | String | No | Updated template ID. |
| `--output` | | String | No | Output format (`json`, `table`, `yaml`, `tsv`). |

## Examples

### Update webhook branch
```bash
terrakube webhook update -o "demo-org" -w "docker-compose-infra" --id "123e4567-e89b-12d3-a456-426614174000" --branch "release/v1"
```
