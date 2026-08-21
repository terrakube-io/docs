# `terrakube module-version create`

Publishes a new release version for a private module.

## Usage

```bash
terrakube module-version create --organization <ORG_NAME_OR_ID> --module <MODULE_NAME_OR_ID> --version <VERSION> [flags]
```

## Flags

| Flag | Short | Type | Required | Description |
| :--- | :--- | :--- | :--- | :--- |
| `--organization` | `-o` | String | Yes | Name or ID of the parent organization. |
| `--module` | `-m` | String | Yes | Name or ID of the parent module. |
| `--version` | | String | Yes | Semantic version string (e.g. `1.0.0`). |
| `--commit` | | String | No | Git commit reference. |
| `--git-tag` | | String | No | Git tag reference. |
| `--output` | | String | No | Output format (`json`, `table`, `yaml`, `tsv`). |

## Examples

### Publish a new module version
```bash
terrakube module-version create \
  -o "demo-org" \
  -m "terraform-aws-vpc" \
  --version "1.2.0" \
  --git-tag "v1.2.0" \
  --output json
```
