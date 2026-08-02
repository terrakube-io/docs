# `terrakube variable create`

Adds a variable to a workspace.

## Usage

```bash
terrakube variable create --organization <ORG> --workspace <WS> --key <KEY> --value <VALUE> --category <CATEGORY> [flags]
```

## Flags

| Flag | Short | Type | Required | Description |
| :--- | :--- | :--- | :--- | :--- |
| `--organization` | `-o` | String | Yes | Organization ID or name. |
| `--workspace` | `-w` | String | Yes | Workspace ID or name. |
| `--key` | `-k` | String | Yes | Variable key name. |
| `--value` | `-v` | String | Yes | Variable value string. |
| `--category` | `-c` | String | Yes | Variable category (`ENV` or `TERRAFORM`). |
| `--hcl` | | Bool | No | Parse value as HCL structure. |
| `--sensitive` | | Bool | No | Hide variable value in console logs. |
| `--description` | `-d` | String | No | Variable description. |

## Examples

```bash
terrakube variable create -o "$ORG_ID" -w "$WS_ID" --key "AWS_REGION" --value "us-west-2" --category "ENV" --output json
```
