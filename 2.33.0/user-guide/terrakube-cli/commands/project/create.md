# `terrakube project create`

Creates a project within an organization.

## Usage

```bash
terrakube project create --organization <ORG> --name <NAME> [flags]
```

## Flags

| Flag | Short | Type | Required | Description |
| :--- | :--- | :--- | :--- | :--- |
| `--organization` | `-o` | String | Yes | Organization ID or name. |
| `--name` | `-n` | String | Yes | Project name. |
| `--description` | `-d` | String | No | Project description. |

## Examples

```bash
terrakube project create -o "$ORG_ID" \
  --name "core-infrastructure" \
  --description "Core cloud setup" \
  --output json
```
