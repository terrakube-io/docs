# `terrakube module-version list`

Lists all versions available for a private module.

## Usage

```bash
terrakube module-version list --organization <ORG_NAME_OR_ID> --module <MODULE_NAME_OR_ID> [flags]
```

## Flags

| Flag | Short | Type | Required | Description |
| :--- | :--- | :--- | :--- | :--- |
| `--organization` | `-o` | String | Yes | Name or ID of the parent organization. |
| `--module` | `-m` | String | Yes | Name or ID of the parent module. |
| `--output` | | String | No | Output format (`json`, `table`, `yaml`, `tsv`). |

## Examples

### List module versions
```bash
terrakube module-version list -o "demo-org" -m "terraform-aws-vpc" --output table
```
