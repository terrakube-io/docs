# `terrakube vcs get`

Retrieves details of a specific VCS connection.

## Usage

```bash
terrakube vcs get --organization <ORG_NAME_OR_ID> --id <VCS_ID> [flags]
```

## Flags

| Flag | Short | Type | Required | Description |
| :--- | :--- | :--- | :--- | :--- |
| `--organization` | `-o` | String | Yes | Name or ID of the parent organization. |
| `--id` | | String | Yes | UUID of the VCS provider. |
| `--output` | | String | No | Output format (`json`, `table`, `yaml`, `tsv`). |

## Examples

### Get VCS provider details
```bash
terrakube vcs get -o "demo-org" --id "123e4567-e89b-12d3-a456-426614174000" --output json
```
