# `terrakube template update`

Updates template parameters or base64-encoded flow content.

## Usage

```bash
terrakube template update --organization <ORG> --id <TPL_ID> [flags]
```

## Examples

```bash
terrakube template update -o "$ORG_ID" \
  --id "$TPL_ID" \
  --name "custom-plan-apply" \
  --description "Updated plan and apply flow" \
  --version "1.0.0" \
  --content "$TCL_B64" \
  --output json
```
