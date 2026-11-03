# `terrakube collection-item update`

Updates a collection item.

## Usage

```bash
terrakube collection-item update --organization <ORG> --collection <COL> --id <ITEM_ID> [flags]
```

## Examples

```bash
terrakube collection-item update -o "$ORG_ID" \
  --collection "$COL_ID" \
  --id "$ITEM_ID" \
  --key "COMMON_ENV" \
  --value "staging" \
  --category "ENV" \
  --output json
```
