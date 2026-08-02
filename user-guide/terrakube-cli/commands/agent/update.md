# `terrakube agent update`

Updates agent endpoint URL or configuration properties.

## Usage

```bash
terrakube agent update --organization <ORG> --id <AGT_ID> --url <URL> [flags]
```

## Examples

```bash
terrakube agent update -o "$ORG_ID" \
  --id "$AGT_ID" \
  --name "agent01" \
  --url "https://localhost:8181" \
  --description "Updated agent URL" \
  --output json
```
