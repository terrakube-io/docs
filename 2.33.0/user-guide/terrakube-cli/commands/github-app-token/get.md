# `terrakube github-app-token get`

Retrieves details of a GitHub App token.

## Usage

```bash
terrakube github-app-token get --id <TOKEN_ID> [flags]
```

## Flags

| Flag | Short | Type | Required | Description |
| :--- | :--- | :--- | :--- | :--- |
| `--id` | | String | Yes | UUID of the token. |
| `--output` | | String | No | Output format (`json`, `table`, `yaml`, `tsv`). |

## Examples

### Get token details
```bash
terrakube github-app-token get --id "123e4567-e89b-12d3-a456-426614174000" --output json
```
