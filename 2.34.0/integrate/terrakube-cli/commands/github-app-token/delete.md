# `terrakube github-app-token delete`

Deletes a GitHub App token.

## Usage

```bash
terrakube github-app-token delete --id <TOKEN_ID> [flags]
```

## Flags

| Flag | Short | Type | Required | Description |
| :--- | :--- | :--- | :--- | :--- |
| `--id` | | String | Yes | UUID of the token to delete. |

## Examples

### Delete a token
```bash
terrakube github-app-token delete --id "123e4567-e89b-12d3-a456-426614174000"
```
