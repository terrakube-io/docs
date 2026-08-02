# `terrakube workspace-tag delete`

Removes tag association from a workspace.

## Usage

```bash
terrakube workspace-tag delete --organization <ORG> --workspace <WS> --id <WSTAG_ID>
```

## Flags

| Flag | Short | Type | Required | Description |
| :--- | :--- | :--- | :--- | :--- |
| `--organization` | `-o` | String | Yes | Organization ID or name. |
| `--workspace` | `-w` | String | Yes | Workspace ID or name. |
| `--id` | | String | Yes | Workspace-Tag association ID. |

## Examples

```bash
terrakube workspace-tag delete -o "$ORG_ID" -w "$WS_ID" --id "$WSTAG_ID"
```
