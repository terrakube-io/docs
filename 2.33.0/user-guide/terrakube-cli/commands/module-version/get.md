# `terrakube module-version get`

Retrieves details of a module version.

## Usage

```bash
terrakube module-version get --organization <ORG_NAME_OR_ID> --module <MODULE_NAME_OR_ID> --id <VERSION_ID> [flags]
```

## Flags

| Flag | Short | Type | Required | Description |
| :--- | :--- | :--- | :--- | :--- |
| `--organization` | `-o` | String | Yes | Name or ID of the parent organization. |
| `--module` | `-m` | String | Yes | Name or ID of the parent module. |
| `--id` | | String | Yes | UUID of the module version. |
| `--output` | | String | No | Output format (`json`, `table`, `yaml`, `tsv`). |

## Examples

### Get module version details
```bash
terrakube module-version get -o "demo-org" -m "terraform-aws-vpc" --id "123e4567-e89b-12d3-a456-426614174000" --output json
```
