# `terrakube tag get`

Retrieves details of a specific organization tag.

## Usage

```bash
terrakube tag get --organization <ORG_NAME_OR_ID> --id <TAG_ID> [flags]
```

## Flags

| Flag | Short | Type | Required | Description |
| :--- | :--- | :--- | :--- | :--- |
| `--organization` | `-o` | String | Yes | Name or ID of the parent organization. |
| `--id` | | String | Yes | UUID of the tag. |
| `--output` | | String | No | Output format (`json`, `table`, `yaml`, `tsv`). |

## Examples

### Get tag details
```bash
terrakube tag get -o "demo-org" --id "456e7890-e89b-12d3-a456-426614174000" --output json
```
