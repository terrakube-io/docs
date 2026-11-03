# `terrakube address create`

Adds a new resource address entry to a job.

## Usage

```bash
terrakube address create --organization <ORG_NAME_OR_ID> --job-id <JOB_ID> --name <NAME> --type <TYPE> [flags]
```

## Flags

| Flag | Short | Type | Required | Description |
| :--- | :--- | :--- | :--- | :--- |
| `--organization` | `-o` | String | Yes | Name or ID of the parent organization. |
| `--job-id` | `-j` | String | Yes | ID of the parent job. |
| `--name` | `-n` | String | Yes | Resource address name. |
| `--type` | | String | Yes | Resource type. |
| `--output` | | String | No | Output format (`json`, `table`, `yaml`, `tsv`). |

## Examples

### Create an address
```bash
terrakube address create \
  -o "demo-org" \
  -j "12" \
  --name "azurerm_resource_group.rg" \
  --type "azurerm_resource_group" \
  --output json
```
