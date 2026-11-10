# `terrakube action create`

Registers a new workspace UI action button.

## Usage

```bash
terrakube action create --name <NAME> [flags]
```

## Flags

| Flag | Short | Type | Required | Description |
| :--- | :--- | :--- | :--- | :--- |
| `--name` | `-n` | String | Yes | Unique action name identifier. |
| `--label` | | String | No | Display label in the UI. |
| `--description` | `-d` | String | No | Action description. |
| `--category` | | String | No | Action category grouping. |
| `--action` | | String | No | Action identifier or payload handler. |
| `--active` | | Bool | No | Whether the action is active. |
| `--type` | | String | No | Action execution type. |
| `--version` | | String | No | Action version. |
| `--display-criteria` | | String | No | JSON/TCL criteria determining when to display the action. |
| `--output` | | String | No | Output format (`json`, `table`, `yaml`, `tsv`). |

## Examples

### Register an action
```bash
terrakube action create \
  --name "restart-vm" \
  --label "Restart VM" \
  --description "Restart Azure Virtual Machine" \
  --category "Azure" \
  --active \
  --output json
```
