# `terrakube vcs list`

Lists all VCS provider connections configured in an organization.

## Usage

```bash
terrakube vcs list --organization <ORG_NAME_OR_ID> [flags]
```

## Flags

| Flag | Short | Type | Required | Description |
| :--- | :--- | :--- | :--- | :--- |
| `--organization` | `-o` | String | Yes | Name or ID of the parent organization. |
| `--output` | | String | No | Output format (`json`, `table`, `yaml`, `tsv`). |

## Examples

### List VCS providers
```bash
terrakube vcs list -o "demo-org" --output table
```
