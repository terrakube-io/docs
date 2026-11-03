# `terrakube vcs update`

Updates attributes of a VCS provider connection.

## Usage

```bash
terrakube vcs update --organization <ORG_NAME_OR_ID> --id <VCS_ID> [flags]
```

## Flags

| Flag | Short | Type | Required | Description |
| :--- | :--- | :--- | :--- | :--- |
| `--organization` | `-o` | String | Yes | Name or ID of the parent organization. |
| `--id` | | String | Yes | UUID of the VCS provider. |
| `--name` | `-n` | String | No | Updated name. |
| `--description` | `-d` | String | No | Updated description. |
| `--client-id` | | String | No | Updated OAuth client ID. |
| `--client-secret` | | String | No | Updated OAuth client secret. |
| `--endpoint` | | String | No | Updated endpoint URL. |
| `--vcs-api-url` | | String | No | Updated API URL. |
| `--output` | | String | No | Output format (`json`, `table`, `yaml`, `tsv`). |

## Examples

### Update VCS description and API URL
```bash
terrakube vcs update \
  -o "demo-org" \
  --id "123e4567-e89b-12d3-a456-426614174000" \
  --description "Updated Production VCS" \
  --vcs-api-url "https://api.github.com" \
  --output table
```
