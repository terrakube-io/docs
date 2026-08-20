# `terrakube module-version update`

Updates attributes of a module version.

## Usage

```bash
terrakube module-version update --organization <ORG_NAME_OR_ID> --module <MODULE_NAME_OR_ID> --id <VERSION_ID> [flags]
```

## Flags

| Flag | Short | Type | Required | Description |
| :--- | :--- | :--- | :--- | :--- |
| `--organization` | `-o` | String | Yes | Name or ID of the parent organization. |
| `--module` | `-m` | String | Yes | Name or ID of the parent module. |
| `--id` | | String | Yes | UUID of the module version. |
| `--version` | | String | No | Updated version string. |
| `--commit` | | String | No | Updated commit reference. |
| `--git-tag` | | String | No | Updated git tag reference. |
| `--output` | | String | No | Output format (`json`, `table`, `yaml`, `tsv`). |

## Examples

### Update module version commit
```bash
terrakube module-version update \
  -o "demo-org" \
  -m "terraform-aws-vpc" \
  --id "123e4567-e89b-12d3-a456-426614174000" \
  --commit "a1b2c3d"
```
