# `terrakube workspace-tag list`

Lists tags associated with a workspace.

## Usage

```bash
terrakube workspace-tag list --organization <ORG> --workspace <WS>
```

## Flags

| Flag | Short | Type | Required | Description |
| :--- | :--- | :--- | :--- | :--- |
| `--organization` | `-o` | String | Yes | Organization ID or name. |
| `--workspace` | `-w` | String | Yes | Workspace ID or name. |

## Examples

```bash
terrakube workspace-tag list -o "$ORG_ID" -w "$WS_ID" --output json
```
