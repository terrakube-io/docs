# `terrakube provider create`

Registers a new provider in the private registry.

## Usage

```bash
terrakube provider create --organization <ORG> --name <NAME> [flags]
```

## Flags

| Flag | Short | Type | Required | Description |
| :--- | :--- | :--- | :--- | :--- |
| `--organization` | `-o` | String | Yes | Organization ID or name. |
| `--name` | `-n` | String | Yes | Provider name (e.g. `random`). |
| `--description` | `-d` | String | No | Description of the provider. |

## Examples

```bash
terrakube provider create -o "$ORG_ID" --name "random1234" --description "Custom random provider" --output json
```
