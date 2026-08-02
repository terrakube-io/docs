# `terrakube collection update`

Updates collection attributes or priority.

## Usage

```bash
terrakube collection update --organization <ORG> --id <COL_ID> [flags]
```

## Examples

```bash
terrakube collection update -o "$ORG_ID" \
  --id "$COL_ID" \
  --name "shared-vars" \
  --description "Updated collection description" \
  --priority 20 \
  --output json
```
