# `terrakube variable list`

Lists all variables configured on a workspace.

## Usage

```bash
terrakube variable list --organization <ORG> --workspace <WS> [flags]
```

## Flags

| Flag | Short | Type | Required | Description |
| :--- | :--- | :--- | :--- | :--- |
| `--organization` | `-o` | String | Yes | Organization ID or name. |
| `--workspace` | `-w` | String | Yes | Workspace ID or name. |
| `--output` | | String | No | Output format (`json`, `table`, `yaml`, `tsv`). |

## Examples

```bash
terrakube variable list -o "$ORG_ID" -w "$WS_ID" --output json
```
