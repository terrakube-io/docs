# `terrakube workspace-tag create`

Associates an organization tag with a workspace.

## Usage

```bash
terrakube workspace-tag create --organization <ORG> --workspace <WS> --tag-id <TAG_ID>
```

## Flags

| Flag | Short | Type | Required | Description |
| :--- | :--- | :--- | :--- | :--- |
| `--organization` | `-o` | String | Yes | Organization ID or name. |
| `--workspace` | `-w` | String | Yes | Workspace ID or name. |
| `--tag-id` | | String | Yes | Organization Tag ID. |

## Examples

```bash
terrakube workspace-tag create -o "$ORG_ID" -w "$WS_ID" --tag-id "$TAG_ID" --output json
```
