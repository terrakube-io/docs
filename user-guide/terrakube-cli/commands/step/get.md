# `terrakube step get`

Retrieves details and log outputs for a specific job step.

## Usage

```bash
terrakube step get --organization <ORG_NAME_OR_ID> --job-id <JOB_ID> --id <STEP_ID> [flags]
```

## Flags

| Flag | Short | Type | Required | Description |
| :--- | :--- | :--- | :--- | :--- |
| `--organization` | `-o` | String | Yes | Name or ID of the parent organization. |
| `--job-id` | `-j` | String | Yes | ID of the parent job. |
| `--id` | | String | Yes | UUID of the step. |
| `--output` | | String | No | Output format (`json`, `table`, `yaml`, `tsv`). |

## Examples

### Get step details
```bash
terrakube step get -o "demo-org" -j "12" --id "123e4567-e89b-12d3-a456-426614174000" --output json
```
