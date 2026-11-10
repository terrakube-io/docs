# `terrakube webhook-event create`

Creates a new webhook event trigger rule.

## Usage

```bash
terrakube webhook-event create --organization <ORG_NAME_OR_ID> --workspace <WORKSPACE_NAME_OR_ID> --webhook-id <WEBHOOK_ID> [flags]
```

## Flags

| Flag | Short | Type | Required | Description |
| :--- | :--- | :--- | :--- | :--- |
| `--organization` | `-o` | String | Yes | Name or ID of the parent organization. |
| `--workspace` | `-w` | String | Yes | Name or ID of the parent workspace. |
| `--webhook-id` | | String | Yes | ID of the parent webhook. |
| `--branch` | | String | No | Branch pattern to match. |
| `--event` | | String | No | Event type (e.g. `push`, `pull_request`). |
| `--path` | | String | No | Monorepo path to match. |
| `--path-type` | | String | No | Path matching strategy (`REGEX`, `EXACT`, `GLOB`). |
| `--priority` | | Int | No | Matching priority. |
| `--template-id` | | String | No | Template ID to trigger. |
| `--pr-workflow-enabled` | | Bool | No | Whether PR workflow plan execution is enabled. |
| `--pr-apply-enabled` | | Bool | No | Whether PR auto-apply is enabled. |
| `--output` | | String | No | Output format (`json`, `table`, `yaml`, `tsv`). |

## Examples

### Create a webhook event rule
```bash
terrakube webhook-event create \
  -o "demo-org" \
  -w "docker-compose-infra" \
  --webhook-id "123e4567-e89b-12d3-a456-426614174000" \
  --branch "main" \
  --event "push" \
  --path-type "GLOB" \
  --path "terraform/**" \
  --pr-workflow-enabled \
  --output json
```
