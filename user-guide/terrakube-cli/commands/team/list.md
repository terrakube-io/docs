# `terrakube team list`

Lists teams in an organization.

## Usage

```bash
terrakube team list --organization <ORG> [flags]
```

## Flags

| Flag | Short | Type | Required | Description |
| :--- | :--- | :--- | :--- | :--- |
| `--organization` | `-o` | String | Yes | Parent Organization name or ID. |
| `--output` | | String | No | Output format (`json`, `table`, `yaml`, `tsv`). |

## Examples

```bash
terrakube team list -o "$ORG_ID" --output table
```
