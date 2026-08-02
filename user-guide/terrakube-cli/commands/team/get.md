# `terrakube team get`

Retrieves details and permissions for a specific team.

## Usage

```bash
terrakube team get --organization <ORG> --id <TEAM_ID>
```

## Flags

| Flag | Short | Type | Required | Description |
| :--- | :--- | :--- | :--- | :--- |
| `--organization` | `-o` | String | Yes | Organization ID or name. |
| `--id` | | String | Yes | Team ID. |

## Examples

```bash
terrakube team get -o "$ORG_ID" --id "$TEAM_ID" --output json
```
