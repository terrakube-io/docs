# `terrakube organization get`

Retrieves details for a specific organization by ID.

## Usage

```bash
terrakube organization get --id <ORG_ID> [flags]
```

## Flags

| Flag | Short | Type | Required | Description |
| :--- | :--- | :--- | :--- | :--- |
| `--id` | | String | Yes | Unique ID of the organization. |
| `--output` | `-o` | String | No | Output format (`json`, `table`, `yaml`, `tsv`). |

## Examples

```bash
terrakube organization get --id "0a1b2c3d-4e5f-6a7b-8c9d-0e1f2a3b4c5d" --output json
```
