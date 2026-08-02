# `terrakube variable update`

Updates a workspace variable.

## Usage

```bash
terrakube variable update --organization <ORG> --workspace <WS> --id <VAR_ID> --key <KEY> --value <VALUE> --category <CATEGORY>
```

## Flags

| Flag | Short | Type | Required | Description |
| :--- | :--- | :--- | :--- | :--- |
| `--organization` | `-o` | String | Yes | Organization ID or name. |
| `--workspace` | `-w` | String | Yes | Workspace ID or name. |
| `--id` | | String | Yes | Variable ID. |
| `--key` | `-k` | String | Yes | Key name. |
| `--value` | `-v` | String | Yes | Updated value. |
| `--category` | `-c` | String | Yes | Category (`ENV`, `TERRAFORM`). |

## Examples

```bash
terrakube variable update -o "$ORG_ID" -w "$WS_ID" --id "$VAR_ID" --key "test1" --value "a" --category "ENV" --output json
```
