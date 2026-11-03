# `terrakube agent create`

Registers a self-hosted agent endpoint URL with an organization.

## Usage

```bash
terrakube agent create --organization <ORG> --name <NAME> --url <URL> [flags]
```

## Flags

| Flag | Short | Type | Required | Description |
| :--- | :--- | :--- | :--- | :--- |
| `--organization` | `-o` | String | Yes | Organization ID or name. |
| `--name` | `-n` | String | Yes | Agent name (e.g. `agent01`). |
| `--url` | | String | Yes | Agent endpoint URL (e.g. `https://localhost:8080`). |
| `--description` | `-d` | String | No | Agent description. |

## Examples

```bash
terrakube agent create -o "$ORG_ID" \
  --name "agent01" \
  --url "https://localhost:8080" \
  --description "Initial agent" \
  --output json
```
