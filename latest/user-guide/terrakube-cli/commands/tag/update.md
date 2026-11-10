# `terrakube tag update`

Renames an existing organization tag.

## Usage

```bash
terrakube tag update --organization <ORG_NAME_OR_ID> --id <TAG_ID> --name <NEW_NAME> [flags]
```

## Flags

| Flag | Short | Type | Required | Description |
| :--- | :--- | :--- | :--- | :--- |
| `--organization` | `-o` | String | Yes | Name or ID of the parent organization. |
| `--id` | | String | Yes | UUID of the tag. |
| `--name` | `-n` | String | Yes | Updated tag name. |
| `--output` | | String | No | Output format (`json`, `table`, `yaml`, `tsv`). |

## Examples

### Rename tag
```bash
terrakube tag update -o "demo-org" --id "456e7890-e89b-12d3-a456-426614174000" --name "staging" --output table
```
