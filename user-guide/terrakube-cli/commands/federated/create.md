# `terrakube federated create`

Registers an OIDC federated identity provider credential.

## Usage

```bash
terrakube federated create --name <NAME> --issuer-url <ISSUER_URL> --audience <AUDIENCE> [flags]
```

## Flags

| Flag | Short | Type | Required | Description |
| :--- | :--- | :--- | :--- | :--- |
| `--name` | `-n` | String | Yes | Name for federated identity (e.g. `github-actions`). |
| `--issuer-url` | | String | Yes | OIDC Token Issuer URL (e.g. `https://token.actions.githubusercontent.com`). |
| `--audience` | | String | Yes | Expected audience string (e.g. `api://Terrakube`). |

## Examples

```bash
terrakube federated create \
  --name "github-actions" \
  --issuer-url "https://token.actions.githubusercontent.com" \
  --audience "api://Terrakube" \
  --output json
```
