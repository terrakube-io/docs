# `terrakube step update`

Updates the status or output of a job step.

## Usage

```bash
terrakube step update --organization <ORG_NAME_OR_ID> --job-id <JOB_ID> --id <STEP_ID> [flags]
```

## Flags

| Flag | Short | Type | Required | Description |
| :--- | :--- | :--- | :--- | :--- |
| `--organization` | `-o` | String | Yes | Name or ID of the parent organization. |
| `--job-id` | `-j` | String | Yes | ID of the parent job. |
| `--id` | | String | Yes | UUID of the step. |
| `--name` | `-n` | String | No | Step name. |
| `--status` | | String | No | Step status (`completed`, `failed`, `running`). |
| `--output` | | String | No | Output log content or format. |

## Examples

### Update step status
```bash
terrakube step update -o "demo-org" -j "12" --id "123e4567-e89b-12d3-a456-426614174000" --status "completed"
```
