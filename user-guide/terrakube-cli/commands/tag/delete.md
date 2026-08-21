# `terrakube tag delete`

Deletes an organization tag.

## Usage

```bash
terrakube tag delete --organization <ORG_NAME_OR_ID> --id <TAG_ID> [flags]
```

## Flags

| Flag | Short | Type | Required | Description |
| :--- | :--- | :--- | :--- | :--- |
| `--organization` | `-o` | String | Yes | Name or ID of the parent organization. |
| `--id` | | String | Yes | UUID of the tag to delete. |

## Examples

### Delete a tag
```bash
terrakube tag delete -o "demo-org" --id "456e7890-e89b-12d3-a456-426614174000"
```
