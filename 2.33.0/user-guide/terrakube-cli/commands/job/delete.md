# `terrakube job delete`

Deletes or cancels an execution job.

## Usage

```bash
terrakube job delete --organization <ORG_NAME_OR_ID> --id <JOB_ID> [flags]
```

## Flags

| Flag | Short | Type | Required | Description |
| :--- | :--- | :--- | :--- | :--- |
| `--organization` | `-o` | String | Yes | Name or ID of the parent organization. |
| `--id` | | String | Yes | UUID/ID of the job. |

## Examples

### Delete a job
```bash
terrakube job delete -o "demo-org" --id "12"
```
