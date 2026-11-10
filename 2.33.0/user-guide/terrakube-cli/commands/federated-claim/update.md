# `terrakube federated-claim update`

Updates a federated identity claim rule.

## Usage

```bash
terrakube federated-claim update --federated <FED> --id <CLAIM_ID> --claim-key <KEY> --claim-value <VAL> [flags]
```

## Examples

```bash
terrakube federated-claim update \
  --federated "$FED_ID" \
  --id "$CLAIM_ID" \
  --claim-key "repository_owner" \
  --claim-value "terrakube-org" \
  --output json
```
