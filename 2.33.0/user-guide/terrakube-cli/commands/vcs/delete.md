# `terrakube vcs delete`

Deletes a VCS provider connection.

## Usage

```bash
terrakube vcs delete --organization <ORG_NAME_OR_ID> --id <VCS_ID> [flags]
```

## Flags

| Flag | Short | Type | Required | Description |
| :--- | :--- | :--- | :--- | :--- |
| `--organization` | `-o` | String | Yes | Name or ID of the parent organization. |
| `--id` | | String | Yes | UUID of the VCS provider to delete. |

## Examples

### Delete a VCS provider
```bash
terrakube vcs delete -o "demo-org" --id "123e4567-e89b-12d3-a456-426614174000"
```
