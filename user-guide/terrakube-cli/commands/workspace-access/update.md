# `terrakube workspace-access update`

Updates workspace access rule permissions.

## Usage

```bash
terrakube workspace-access update --organization <ORG> --workspace <WS> --id <ACCESS_ID> [flags]
```

## Examples

```bash
terrakube workspace-access update -o "$ORG_ID" -w "$WS_ID" \
  --id "$ACCESS_ID" \
  --name "TERRAKUBE_ADMIN" \
  --manage-state=false \
  --manage-workspace \
  --manage-job \
  --output json
```
