# `terrakube github-app-token create`

Registers a new GitHub App token.

## Usage

```bash
terrakube github-app-token create --app-id <APP_ID> --installation-id <INSTALL_ID> --owner <OWNER> [flags]
```

## Flags

| Flag | Short | Type | Required | Description |
| :--- | :--- | :--- | :--- | :--- |
| `--app-id` | | String | Yes | GitHub App ID. |
| `--installation-id` | | String | Yes | GitHub App installation ID. |
| `--owner` | | String | Yes | GitHub App owner/organization. |
| `--expires-at` | | String | No | Token expiration timestamp. |
| `--output` | | String | No | Output format (`json`, `table`, `yaml`, `tsv`). |

## Examples

### Create a GitHub App token
```bash
terrakube github-app-token create \
  --app-id "123456" \
  --installation-id "98765432" \
  --owner "terrakube-io" \
  --output json
```
