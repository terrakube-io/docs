# `terrakube workspace delete`

Permanently deletes a workspace from an organization.

## Usage

```bash
terrakube workspace delete --organization <ORG> --id <WS_ID>
```

## Flags

| Flag | Short | Type | Required | Description |
| :--- | :--- | :--- | :--- | :--- |
| `--organization` | `-o` | String | Yes | Organization ID or name. |
| `--id` | | String | Yes | Workspace ID. |

## Examples

```bash
terrakube workspace delete -o "$ORG_ID" --id "$WS_ID"
```
