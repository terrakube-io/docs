# `terrakube team update`

Updates role permissions on an existing team.

## Usage

```bash
terrakube team update --organization <ORG> --id <TEAM_ID> [flags]
```

## Flags

| Flag | Short | Type | Required | Description |
| :--- | :--- | :--- | :--- | :--- |
| `--organization` | `-o` | String | Yes | Organization ID or name. |
| `--id` | | String | Yes | Team ID to update. |
| `--name` | `-n` | String | No | Team name. |
| `--role` | | String | No | Role name. |
| `--manage-workspace` | | Bool | No | Set workspace management permission. |

## Examples

```bash
terrakube team update -o "$ORG_ID" --id "$TEAM_ID" --name "TERRAKUBE_ADMIN" --role "Admin" --output json
```
