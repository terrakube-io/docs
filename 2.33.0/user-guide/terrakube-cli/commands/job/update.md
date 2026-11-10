# `terrakube job update`

Updates attributes or status of a job (e.g. approving a pending approval job).

## Usage

```bash
terrakube job update --organization <ORG_NAME_OR_ID> --id <JOB_ID> [flags]
```

## Flags

| Flag | Short | Type | Required | Description |
| :--- | :--- | :--- | :--- | :--- |
| `--organization` | `-o` | String | Yes | Name or ID of the parent organization. |
| `--id` | | String | Yes | UUID of the job. |
| `--status` | | String | No | Updated status (e.g. `approved`, `rejected`). |
| `--output` | | String | No | Output format (`json`, `table`, `yaml`, `tsv`). |

## Examples

### Approve a job waiting for approval
```bash
terrakube job update -o "demo-org" --id "12" --status "approved" --output table
```
