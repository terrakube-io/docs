# `terrakube federated-claim create`

Adds a claim rule to a federated identity credential.

## Usage

```bash
terrakube federated-claim create --federated <FED> --claim-key <KEY> --claim-value <VAL> [flags]
```

## Flags

| Flag | Short | Type | Required | Description |
| :--- | :--- | :--- | :--- | :--- |
| `--federated` | | String | Yes | Federated credential ID or name. |
| `--claim-key` | | String | Yes | OIDC claim key (e.g. `repository_owner`, `sub`). |
| `--claim-value` | | String | Yes | OIDC claim value matching criteria. |

## Examples

```bash
terrakube federated-claim create \
  --federated "$FED_ID" \
  --claim-key "repository_owner" \
  --claim-value "terrakube-io" \
  --output json
```
