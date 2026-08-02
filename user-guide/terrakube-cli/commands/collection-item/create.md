# `terrakube collection-item create`

Adds a variable item to a collection.

## Usage

```bash
terrakube collection-item create --organization <ORG> --collection <COL> --key <KEY> --value <VAL> --category <CATEGORY> [flags]
```

## Flags

| Flag | Short | Type | Required | Description |
| :--- | :--- | :--- | :--- | :--- |
| `--organization` | `-o` | String | Yes | Organization ID or name. |
| `--collection` | | String | Yes | Collection ID or name. |
| `--key` | `-k` | String | Yes | Key name. |
| `--value` | `-v` | String | Yes | Value string. |
| `--category` | `-c` | String | Yes | Category (`ENV`, `TERRAFORM`). |
| `--description` | `-d` | String | No | Description. |

## Examples

```bash
terrakube collection-item create -o "$ORG_ID" \
  --collection "$COL_ID" \
  --key "COMMON_ENV" \
  --value "prod" \
  --category "ENV" \
  --description "Collection item description" \
  --output json
```
