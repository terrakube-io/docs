# `terrakube team delete`

Deletes a team from an organization.

## Usage

```bash
terrakube team delete --organization <ORG> --id <TEAM_ID>
```

## Flags

| Flag | Short | Type | Required | Description |
| :--- | :--- | :--- | :--- | :--- |
| `--organization` | `-o` | String | Yes | Organization ID or name. |
| `--id` | | String | Yes | Team ID. |

## Examples

```bash
terrakube team delete -o "$ORG_ID" --id "$TEAM_ID"
```
