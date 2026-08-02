# `terrakube ssh update`

Updates SSH key metadata.

## Usage

```bash
terrakube ssh update --organization <ORG> --id <SSH_ID> [flags]
```

## Examples

```bash
terrakube ssh update -o "$ORG_ID" \
  --id "$SSH_ID" \
  --name "ssh-prod" \
  --ssh-type "rsa" \
  --private-key "$DUMMY_KEY" \
  --description "Updated SSH key description" \
  --output json
```
