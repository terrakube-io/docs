# `terrakube step delete`

Deletes an execution step.

## Usage

```bash
terrakube step delete --organization <ORG_NAME_OR_ID> --job-id <JOB_ID> --id <STEP_ID> [flags]
```

## Flags

| Flag | Short | Type | Required | Description |
| :--- | :--- | :--- | :--- | :--- |
| `--organization` | `-o` | String | Yes | Name or ID of the parent organization. |
| `--job-id` | `-j` | String | Yes | ID of the parent job. |
| `--id` | | String | Yes | UUID of the step to delete. |

## Examples

### Delete a step
```bash
terrakube step delete -o "demo-org" -j "12" --id "123e4567-e89b-12d3-a456-426614174000"
```
