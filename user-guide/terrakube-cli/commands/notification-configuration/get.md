# `terrakube notification-configuration get`

Retrieves details of a specific notification configuration.

## Usage

```bash
terrakube notification-configuration get --organization <ORG_NAME_OR_ID> --id <CONFIG_ID> [flags]
```

## Flags

| Flag | Short | Type | Required | Description |
| :--- | :--- | :--- | :--- | :--- |
| `--organization` | `-o` | String | Yes | Name or ID of the parent organization. |
| `--id` | | String | Yes | UUID of the notification configuration. |
| `--output` | | String | No | Output format (`json`, `table`, `yaml`, `tsv`). |

## Examples

### Get notification configuration details
```bash
terrakube notification-configuration get -o "demo-org" --id "270db279-b1d5-4fd3-ab69-2f58e458e0a1" --output json
```
