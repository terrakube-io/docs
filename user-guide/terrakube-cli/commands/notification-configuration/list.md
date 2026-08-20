# `terrakube notification-configuration list`

Lists notification configurations for an organization or workspace.

## Usage

```bash
terrakube notification-configuration list --organization <ORG_NAME_OR_ID> [flags]
```

## Flags

| Flag | Short | Type | Required | Description |
| :--- | :--- | :--- | :--- | :--- |
| `--organization` | `-o` | String | Yes | Name or ID of the parent organization. |
| `--workspace` | `-w` | String | No | Name or ID of the workspace (to list workspace-level configurations). |
| `--output` | | String | No | Output format (`json`, `table`, `yaml`, `tsv`). |

## Examples

### List organization-level notification configurations
```bash
terrakube notification-configuration list -o "demo-org" --output table
```

### List workspace-level notification configurations
```bash
terrakube notification-configuration list -o "demo-org" -w "docker-compose-infra" --output json
```
