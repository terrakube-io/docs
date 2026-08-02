# `terrakube project-access update`

Updates permissions for a project access rule.

## Usage

```bash
terrakube project-access update --organization <ORG> --project <PRJ> --id <PRJ_ACC_ID> [flags]
```

## Examples

```bash
terrakube project-access update -o "$ORG_ID" \
  --project "$PRJ_ID" \
  --id "$PRJ_ACC_ID" \
  --name "TERRAKUBE_ADMIN" \
  --manage-state=false \
  --manage-workspace \
  --manage-job \
  --output json
```
