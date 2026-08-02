# `terrakube collection-reference create`

Links a variable collection to a target workspace.

## Usage

```bash
terrakube collection-reference create --organization <ORG> --collection <COL> --workspace <WS> [flags]
```

## Flags

| Flag | Short | Type | Required | Description |
| :--- | :--- | :--- | :--- | :--- |
| `--organization` | `-o` | String | Yes | Organization ID or name. |
| `--collection` | | String | Yes | Collection ID or name. |
| `--workspace` | `-w` | String | Yes | Target Workspace ID or name. |
| `--description` | `-d` | String | No | Description. |

## Examples

```bash
terrakube collection-reference create -o "$ORG_ID" \
  --collection "$COL_ID" \
  -w "$WS_ID" \
  --description "Collection reference to workspace" \
  --output json
```
