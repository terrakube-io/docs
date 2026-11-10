# `terrakube project update`

Updates a project.

## Usage

```bash
terrakube project update --organization <ORG> --id <PRJ_ID> [flags]
```

## Examples

```bash
terrakube project update -o "$ORG_ID" \
  --id "$PRJ_ID" \
  --name "core-infrastructure" \
  --description "Updated project description" \
  --output json
```
