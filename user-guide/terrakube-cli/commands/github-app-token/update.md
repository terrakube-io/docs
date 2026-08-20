# `terrakube github-app-token update`

Updates a GitHub App token.

## Usage

```bash
terrakube github-app-token update --id <TOKEN_ID> [flags]
```

## Flags

| Flag | Short | Type | Required | Description |
| :--- | :--- | :--- | :--- | :--- |
| `--id` | | String | Yes | UUID of the token. |
| `--app-id` | | String | No | Updated App ID. |
| `--installation-id` | | String | No | Updated Installation ID. |
| `--owner` | | String | No | Updated Owner. |
| `--expires-at` | | String | No | Updated expiration timestamp. |
| `--output` | | String | No | Output format (`json`, `table`, `yaml`, `tsv`). |

## Examples

### Update token owner
```bash
terrakube github-app-token update --id "123e4567-e89b-12d3-a456-426614174000" --owner "new-org"
```
