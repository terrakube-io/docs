# `terrakube action update`

Updates attributes of a registered UI action.

## Usage

```bash
terrakube action update --id <ACTION_ID> [flags]
```

## Flags

| Flag | Short | Type | Required | Description |
| :--- | :--- | :--- | :--- | :--- |
| `--id` | | String | Yes | UUID of the action. |
| `--name` | `-n` | String | No | Updated action name. |
| `--label` | | String | No | Updated label. |
| `--description` | `-d` | String | No | Updated description. |
| `--active` | | Bool | No | Set active/inactive status. |
| `--output` | | String | No | Output format (`json`, `table`, `yaml`, `tsv`). |

## Examples

### Update an action label
```bash
terrakube action update --id "123e4567-e89b-12d3-a456-426614174000" --label "Reboot VM Now"
```
