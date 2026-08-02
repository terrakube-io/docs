# `terrakube collection create`

Creates a new variable collection.

## Usage

```bash
terrakube collection create --organization <ORG> --name <NAME> [flags]
```

## Flags

| Flag | Short | Type | Required | Description |
| :--- | :--- | :--- | :--- | :--- |
| `--organization` | `-o` | String | Yes | Organization ID or name. |
| `--name` | `-n` | String | Yes | Collection name. |
| `--description` | `-d` | String | No | Description. |
| `--priority` | | Int | No | Evaluation priority integer (e.g. `10`). |

## Examples

```bash
terrakube collection create -o "$ORG_ID" \
  --name "shared-vars" \
  --description "Shared cloud settings" \
  --priority 10 \
  --output json
```
