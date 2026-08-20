# `terrakube address delete`

Deletes a resource address from a job.

## Usage

```bash
terrakube address delete --organization <ORG_NAME_OR_ID> --job-id <JOB_ID> --id <ADDRESS_ID> [flags]
```

## Flags

| Flag | Short | Type | Required | Description |
| :--- | :--- | :--- | :--- | :--- |
| `--organization` | `-o` | String | Yes | Name or ID of the parent organization. |
| `--job-id` | `-j` | String | Yes | ID of the parent job. |
| `--id` | | String | Yes | UUID of the address to delete. |

## Examples

### Delete an address
```bash
terrakube address delete -o "demo-org" -j "12" --id "123e4567-e89b-12d3-a456-426614174000"
```
