# `terrakube federated update`

Updates a federated identity credential.

## Usage

```bash
terrakube federated update --id <FED_ID> [flags]
```

## Examples

```bash
terrakube federated update \
  --id "$FED_ID" \
  --name "github-actions" \
  --issuer-url "https://token.actions.githubusercontent.com" \
  --audience "api://TerrakubeV2" \
  --output json
```
