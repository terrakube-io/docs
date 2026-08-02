# `terrakube module create`

Registers a new module in the private module registry.

## Usage

```bash
terrakube module create --organization <ORG> --name <NAME> --provider <PROVIDER> --source <SOURCE> [flags]
```

## Flags

| Flag | Short | Type | Required | Description |
| :--- | :--- | :--- | :--- | :--- |
| `--organization` | `-o` | String | Yes | Organization ID or name. |
| `--name` | `-n` | String | Yes | Module name (e.g. `iam`). |
| `--provider` | | String | Yes | Target cloud provider (e.g. `google`, `aws`, `azurerm`). |
| `--source` | | String | Yes | Git source repository URL. |
| `--description` | `-d` | String | No | Description of the module. |

## Examples

```bash
terrakube module create -o "$ORG_ID" \
  --name "iam" \
  --provider "google" \
  --source "https://github.com/terraform-google-modules/terraform-google-iam" \
  --description "Initial IAM module" \
  --output json
```
