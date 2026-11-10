# `terrakube tag list`

Lists all tags defined within an organization.

## Usage

```bash
terrakube tag list --organization <ORG_NAME_OR_ID> [flags]
```

## Flags

| Flag | Short | Type | Required | Description |
| :--- | :--- | :--- | :--- | :--- |
| `--organization` | `-o` | String | Yes | Name or ID of the parent organization. |
| `--output` | | String | No | Output format (`json`, `table`, `yaml`, `tsv`). |

## Examples

### List tags
```bash
terrakube tag list -o "demo-org" --output table
```
