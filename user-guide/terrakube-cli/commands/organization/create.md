# `terrakube organization create`

Creates a new organization.

## Usage

```bash
terrakube organization create --name <NAME> [flags]
```

## Flags

| Flag | Short | Type | Required | Description |
| :--- | :--- | :--- | :--- | :--- |
| `--name` | `-n` | String | Yes | Name of the organization. |
| `--description` | `-d` | String | No | Description of the organization. |
| `--execution-mode` | | String | No | Execution mode (`remote`, `local`). |
| `--output` | `-o` | String | No | Output format (`json`, `table`, `yaml`, `tsv`). |

## Examples

### Create an organization
```bash
terrakube organization create --name "bats" --description "Initial BATS Org" --execution-mode "remote" --output json
```
