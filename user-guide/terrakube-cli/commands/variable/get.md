# `terrakube variable get`

Retrieves a workspace variable by ID.

## Usage

```bash
terrakube variable get --organization <ORG> --workspace <WS> --id <VAR_ID>
```

## Flags

| Flag | Short | Type | Required | Description |
| :--- | :--- | :--- | :--- | :--- |
| `--organization` | `-o` | String | Yes | Organization ID or name. |
| `--workspace` | `-w` | String | Yes | Workspace ID or name. |
| `--id` | | String | Yes | Variable ID. |

## Examples

```bash
terrakube variable get -o "$ORG_ID" -w "$WS_ID" --id "$VAR_ID" --output json
```
