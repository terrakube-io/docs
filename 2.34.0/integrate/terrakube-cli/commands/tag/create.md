# `terrakube tag create`

Creates a new organization tag.

## Usage

```bash
terrakube tag create --organization <ORG_NAME_OR_ID> --name <NAME> [flags]
```

## Flags

| Flag | Short | Type | Required | Description |
| :--- | :--- | :--- | :--- | :--- |
| `--organization` | `-o` | String | Yes | Name or ID of the parent organization. |
| `--name` | `-n` | String | Yes | Name of the tag. |
| `--output` | | String | No | Output format (`json`, `table`, `yaml`, `tsv`). |

## Examples

### Create an organization tag
```bash
terrakube tag create -o "demo-org" --name "production" --output json
```
