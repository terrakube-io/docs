# `terrakube workspace list`

Lists workspaces in an organization.

## Usage

```bash
terrakube workspace list --organization <ORG> [flags]
```

## Flags

| Flag | Short | Type | Required | Description |
| :--- | :--- | :--- | :--- | :--- |
| `--organization` | `-o` | String | Yes | Organization ID or name. |
| `--filter` | | String | No | Filter expression (e.g. `name==testws`). |
| `--output` | | String | No | Output format (`json`, `table`, `yaml`, `tsv`). |

## Examples

```bash
terrakube workspace list -o "$ORG_ID" --filter "name==networking-prod" --output json
```
