# `terrakube module-version delete`

Deletes a module version.

## Usage

```bash
terrakube module-version delete --organization <ORG_NAME_OR_ID> --module <MODULE_NAME_OR_ID> --id <VERSION_ID> [flags]
```

## Flags

| Flag | Short | Type | Required | Description |
| :--- | :--- | :--- | :--- | :--- |
| `--organization` | `-o` | String | Yes | Name or ID of the parent organization. |
| `--module` | `-m` | String | Yes | Name or ID of the parent module. |
| `--id` | | String | Yes | UUID of the module version to delete. |

## Examples

### Delete a module version
```bash
terrakube module-version delete -o "demo-org" -m "terraform-aws-vpc" --id "123e4567-e89b-12d3-a456-426614174000"
```
