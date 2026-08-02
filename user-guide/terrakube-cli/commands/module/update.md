# `terrakube module update`

Updates a registered module.

## Usage

```bash
terrakube module update --organization <ORG> --id <MOD_ID> [flags]
```

## Examples

```bash
terrakube module update -o "$ORG_ID" \
  --id "$MOD_ID" \
  --name "iam" \
  --provider "google" \
  --source "https://github.com/terraform-google-modules/terraform-google-iam" \
  --description "Updated IAM module description" \
  --output json
```
