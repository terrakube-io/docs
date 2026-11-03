# `terrakube step list`

Lists all execution steps within a specific job.

## Usage

```bash
terrakube step list --organization <ORG_NAME_OR_ID> --job-id <JOB_ID> [flags]
```

## Flags

| Flag | Short | Type | Required | Description |
| :--- | :--- | :--- | :--- | :--- |
| `--organization` | `-o` | String | Yes | Name or ID of the parent organization. |
| `--job-id` | `-j` | String | Yes | ID of the parent job. |
| `--output` | | String | No | Output format (`json`, `table`, `yaml`, `tsv`). |

## Examples

### List job steps
```bash
terrakube step list -o "demo-org" -j "12" --output table
```
