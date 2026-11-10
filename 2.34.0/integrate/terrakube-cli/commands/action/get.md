# `terrakube action get`

Retrieves details of a registered UI action.

## Usage

```bash
terrakube action get --id <ACTION_ID> [flags]
```

## Flags

| Flag | Short | Type | Required | Description |
| :--- | :--- | :--- | :--- | :--- |
| `--id` | | String | Yes | UUID of the action. |
| `--output` | | String | No | Output format (`json`, `table`, `yaml`, `tsv`). |

## Examples

### Get action details
```bash
terrakube action get --id "123e4567-e89b-12d3-a456-426614174000" --output json
```
