# `terrakube webhook list`

Lists all webhooks configured for a workspace.

## Usage

```bash
terrakube webhook list --organization <ORG_NAME_OR_ID> --workspace <WORKSPACE_NAME_OR_ID> [flags]
```

## Flags

| Flag | Short | Type | Required | Description |
| :--- | :--- | :--- | :--- | :--- |
| `--organization` | `-o` | String | Yes | Name or ID of the parent organization. |
| `--workspace` | `-w` | String | Yes | Name or ID of the parent workspace. |
| `--output` | | String | No | Output format (`json`, `table`, `yaml`, `tsv`). |

## Examples

### List webhooks
```bash
terrakube webhook list -o "demo-org" -w "docker-compose-infra" --output table
```
