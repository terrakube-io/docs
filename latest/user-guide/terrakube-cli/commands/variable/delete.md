# `terrakube variable delete`

Deletes a variable from a workspace.

## Usage

```bash
terrakube variable delete --organization <ORG> --workspace <WS> --id <VAR_ID>
```

## Flags

| Flag | Short | Type | Required | Description |
| :--- | :--- | :--- | :--- | :--- |
| `--organization` | `-o` | String | Yes | Organization ID or name. |
| `--workspace` | `-w` | String | Yes | Workspace ID or name. |
| `--id` | | String | Yes | Variable ID to remove. |

## Examples

```bash
terrakube variable delete -o "$ORG_ID" -w "$WS_ID" --id "$VAR_ID"
```
