# `terrakube job list`

Lists jobs within an organization.

## Usage

```bash
terrakube job list --organization <ORG_NAME_OR_ID> [flags]
```

## Flags

| Flag | Short | Type | Required | Description |
| :--- | :--- | :--- | :--- | :--- |
| `--organization` | `-o` | String | Yes | Name or ID of the parent organization. |
| `--output` | | String | No | Output format (`json`, `table`, `yaml`, `tsv`). |

## Examples

### List all organization jobs
```bash
terrakube job list -o "demo-org" --output table
```
