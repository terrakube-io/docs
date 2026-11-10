# `terrakube action delete`

Deletes a UI action registration.

## Usage

```bash
terrakube action delete --id <ACTION_ID> [flags]
```

## Flags

| Flag | Short | Type | Required | Description |
| :--- | :--- | :--- | :--- | :--- |
| `--id` | | String | Yes | UUID of the action to delete. |

## Examples

### Delete an action
```bash
terrakube action delete --id "123e4567-e89b-12d3-a456-426614174000"
```
