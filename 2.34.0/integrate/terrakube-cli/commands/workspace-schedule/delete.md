# `terrakube workspace-schedule delete`

Deletes a workspace schedule.

## Usage

```bash
terrakube workspace-schedule delete --organization <ORG_NAME_OR_ID> --workspace <WORKSPACE_NAME_OR_ID> --id <SCHEDULE_ID> [flags]
```

## Flags

| Flag | Short | Type | Required | Description |
| :--- | :--- | :--- | :--- | :--- |
| `--organization` | `-o` | String | Yes | Name or ID of the parent organization. |
| `--workspace` | `-w` | String | Yes | Name or ID of the parent workspace. |
| `--id` | | String | Yes | UUID of the schedule to delete. |

## Examples

### Delete a schedule
```bash
terrakube workspace-schedule delete -o "demo-org" -w "docker-compose-infra" --id "123e4567-e89b-12d3-a456-426614174000"
```
