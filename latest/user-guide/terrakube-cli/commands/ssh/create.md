# `terrakube ssh create`

Registers an SSH private key for Git authentication.

## Usage

```bash
terrakube ssh create --organization <ORG> --name <NAME> --ssh-type <TYPE> --private-key <KEY> [flags]
```

## Flags

| Flag | Short | Type | Required | Description |
| :--- | :--- | :--- | :--- | :--- |
| `--organization` | `-o` | String | Yes | Organization ID or name. |
| `--name` | `-n` | String | Yes | Name for SSH key entry. |
| `--ssh-type` | | String | Yes | SSH Key type (e.g. `rsa`, `ed25519`). |
| `--private-key` | | String | Yes | Private key string content. |
| `--description` | `-d` | String | No | Description. |

## Examples

```bash
DUMMY_KEY="-----BEGIN RSA PRIVATE KEY-----\n...\n-----END RSA PRIVATE KEY-----\n"

terrakube ssh create -o "$ORG_ID" \
  --name "ssh-prod" \
  --ssh-type "rsa" \
  --private-key "$DUMMY_KEY" \
  --description "Initial SSH key" \
  --output json
```
