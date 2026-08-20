# `terrakube vcs create`

Adds a new VCS provider integration.

## Usage

```bash
terrakube vcs create --organization <ORG_NAME_OR_ID> --name <NAME> --description <DESC> --vcs-type <TYPE> --connection-type <CONN> --client-id <ID> --client-secret <SECRET> --endpoint <URL> --vcs-api-url <API_URL> [flags]
```

## Flags

| Flag | Short | Type | Required | Description |
| :--- | :--- | :--- | :--- | :--- |
| `--organization` | `-o` | String | Yes | Name or ID of the parent organization. |
| `--name` | `-n` | String | Yes | VCS connection name. |
| `--description` | `-d` | String | Yes | VCS description. |
| `--vcs-type` | | String | Yes | VCS type (`GITHUB`, `GITLAB`, `BITBUCKET`, `AZURE_DEVOPS`). |
| `--connection-type` | | String | Yes | Connection type (`OAUTH`, `SSH`). |
| `--client-id` | | String | Yes | OAuth client ID. |
| `--client-secret` | | String | Yes | OAuth client secret. |
| `--endpoint` | | String | Yes | VCS web endpoint URL. |
| `--vcs-api-url` | | String | Yes | VCS API endpoint URL. |
| `--private-key` | | String | No | SSH private key. |
| `--redirect-url` | | String | No | OAuth redirect URL. |
| `--output` | | String | No | Output format (`json`, `table`, `yaml`, `tsv`). |

## Examples

### Create a GitHub VCS integration
```bash
terrakube vcs create \
  -o "demo-org" \
  --name "github-enterprise" \
  --description "Enterprise GitHub VCS" \
  --vcs-type "GITHUB" \
  --connection-type "OAUTH" \
  --client-id "Iv1.xxxxxxxxxxxx" \
  --client-secret "xxxxxxxxxxxxxxxxxxxxxxxx" \
  --endpoint "https://github.com" \
  --vcs-api-url "https://api.github.com" \
  --output json
```
