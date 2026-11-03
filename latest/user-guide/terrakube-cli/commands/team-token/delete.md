# `terrakube team-token delete`

Revokes/deletes an active team API token.

## Usage

```bash
terrakube team-token delete --id <TOKEN_ID> [flags]
```

## Flags

| Flag | Short | Type | Required | Description |
| :--- | :--- | :--- | :--- | :--- |
| `--id` | | String | Yes | UUID/ID of the team token to revoke. |

## Examples

### Delete a team token
```bash
terrakube team-token delete --id "123e4567-e89b-12d3-a456-426614174000"
```
