# `terrakube organization-variable update`

Updates key or value of an organization global variable.

## Usage

```bash
terrakube organization-variable update --organization <ORG> --id <GVAR_ID> --key <KEY> --value <VALUE> --category <CATEGORY>
```

## Examples

```bash
terrakube organization-variable update -o "$ORG_ID" \
  --id "$GVAR_ID" \
  --key "GLOBAL_REGION" \
  --value "us-west-2" \
  --category "ENV" \
  --description "Updated global variable" \
  --output json
```
