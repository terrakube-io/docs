# `terrakube organization update`

Updates properties of an existing organization.

## Usage

```bash
terrakube organization update --id <ORG_ID> [flags]
```

## Flags

| Flag | Short | Type | Required | Description |
| :--- | :--- | :--- | :--- | :--- |
| `--id` | | String | Yes | ID of the organization to update. |
| `--name` | `-n` | String | No | Updated organization name. |
| `--description` | `-d` | String | No | Updated organization description. |
| `--execution-mode` | | String | No | Updated execution mode (`remote`, `local`). |
| `--output` | `-o` | String | No | Output format (`json`, `table`, `yaml`, `tsv`). |

## Examples

```bash
terrakube organization update --id "$ORG_ID" --name "bats" --description "Updated BATS Org Description" --execution-mode "remote" --output json
```
