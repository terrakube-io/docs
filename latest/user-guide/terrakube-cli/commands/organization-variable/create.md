# `terrakube organization-variable create`

Creates a global variable at the organization level.

## Usage

```bash
terrakube organization-variable create --organization <ORG> --key <KEY> --value <VALUE> --category <CATEGORY> [flags]
```

## Flags

| Flag | Short | Type | Required | Description |
| :--- | :--- | :--- | :--- | :--- |
| `--organization` | `-o` | String | Yes | Organization ID or name. |
| `--key` | `-k` | String | Yes | Variable key name. |
| `--value` | `-v` | String | Yes | Variable value string. |
| `--category` | `-c` | String | Yes | Variable category (`ENV` or `TERRAFORM`). |
| `--description` | `-d` | String | No | Description. |

## Examples

```bash
terrakube organization-variable create -o "$ORG_ID" \
  --key "GLOBAL_REGION" \
  --value "us-east-1" \
  --category "ENV" \
  --description "Initial global variable" \
  --output json
```
