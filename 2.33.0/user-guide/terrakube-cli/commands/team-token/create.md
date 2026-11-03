# `terrakube team-token create`

Generates a new API token assigned to a team/group with a specified validity period.

## Usage

```bash
terrakube team-token create --group <GROUP_NAME> [flags]
```

## Flags

| Flag | Short | Type | Required | Description |
| :--- | :--- | :--- | :--- | :--- |
| `--group` | `-g` | String | Yes | Team / Group name. |
| `--description` | `-d` | String | No | Token description or purpose. |
| `--days` | | Int32 | No | Token validity duration in days. |
| `--hours` | | Int32 | No | Token validity duration in hours. |
| `--minutes` | | Int32 | No | Token validity duration in minutes. |
| `--output` | | String | No | Output format (`json`, `table`, `yaml`, `tsv`). |

## Examples

### Create a 30-day team token for DevOps team
```bash
terrakube team-token create \
  --group "devops" \
  --description "CI/CD Deployment Token" \
  --days 30 \
  --output json
```
