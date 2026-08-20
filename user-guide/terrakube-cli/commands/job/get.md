# `terrakube job get`

Retrieves status, output, and metadata of a specific job.

## Usage

```bash
terrakube job get --organization <ORG_NAME_OR_ID> --id <JOB_ID> [flags]
```

## Flags

| Flag | Short | Type | Required | Description |
| :--- | :--- | :--- | :--- | :--- |
| `--organization` | `-o` | String | Yes | Name or ID of the parent organization. |
| `--id` | | String | Yes | UUID of the job. |
| `--output` | | String | No | Output format (`json`, `table`, `yaml`, `tsv`). |

## Examples

### Get job status
```bash
terrakube job get -o "demo-org" --id "12" --output json
```
