# `terrakube address update`

Updates a resource address definition.

## Usage

```bash
terrakube address update --organization <ORG_NAME_OR_ID> --job-id <JOB_ID> --id <ADDRESS_ID> [flags]
```

## Flags

| Flag | Short | Type | Required | Description |
| :--- | :--- | :--- | :--- | :--- |
| `--organization` | `-o` | String | Yes | Name or ID of the parent organization. |
| `--job-id` | `-j` | String | Yes | ID of the parent job. |
| `--id` | | String | Yes | UUID of the address. |
| `--name` | `-n` | String | No | Updated address name. |
| `--type` | | String | No | Updated address type. |
| `--output` | | String | No | Output format (`json`, `table`, `yaml`, `tsv`). |

## Examples

### Update an address name
```bash
terrakube address update -o "demo-org" -j "12" --id "123e4567-e89b-12d3-a456-426614174000" --name "azurerm_resource_group.main"
```
