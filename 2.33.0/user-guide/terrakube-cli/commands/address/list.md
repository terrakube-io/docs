# `terrakube address list`

Lists all resource addresses for a job.

## Usage

```bash
terrakube address list --organization <ORG_NAME_OR_ID> --job-id <JOB_ID> [flags]
```

## Flags

| Flag | Short | Type | Required | Description |
| :--- | :--- | :--- | :--- | :--- |
| `--organization` | `-o` | String | Yes | Name or ID of the parent organization. |
| `--job-id` | `-j` | String | Yes | ID of the parent job. |
| `--output` | | String | No | Output format (`json`, `table`, `yaml`, `tsv`). |

## Examples

### List addresses
```bash
terrakube address list -o "demo-org" -j "12" --output table
```
