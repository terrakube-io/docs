# `terrakube history get`

Retrieves details of a history record.

## Usage

```bash
terrakube history get --organization <ORG_NAME_OR_ID> --workspace <WORKSPACE_NAME_OR_ID> --id <HISTORY_ID> [flags]
```

## Flags

| Flag | Short | Type | Required | Description |
| :--- | :--- | :--- | :--- | :--- |
| `--organization` | `-o` | String | Yes | Name or ID of the parent organization. |
| `--workspace` | `-w` | String | Yes | Name or ID of the parent workspace. |
| `--id` | | String | Yes | UUID of the history entry. |
| `--output` | | String | No | Output format (`json`, `table`, `yaml`, `tsv`). |

## Examples

### Get history details
```bash
terrakube history get -o "demo-org" -w "docker-compose-infra" --id "123e4567-e89b-12d3-a456-426614174000" --output json
```
