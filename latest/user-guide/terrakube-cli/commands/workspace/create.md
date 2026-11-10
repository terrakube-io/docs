# `terrakube workspace create`

Creates a new workspace configured with OpenTofu or Terraform settings.

## Usage

```bash
terrakube workspace create --organization <ORG> --name <NAME> --source <SOURCE_URL> --branch <BRANCH> --folder <FOLDER> --iac-type <TYPE> --iac-version <VERSION> [flags]
```

## Flags

| Flag | Short | Type | Required | Description |
| :--- | :--- | :--- | :--- | :--- |
| `--organization` | `-o` | String | Yes | Organization ID or name. |
| `--name` | `-n` | String | Yes | Workspace name. |
| `--source` | | String | Yes | VCS Git repository URL. |
| `--branch` | | String | Yes | Repository branch (e.g. `main`). |
| `--folder` | | String | Yes | Directory path inside repository (e.g. `/`). |
| `--iac-type` | | String | Yes | IaC tool type (`tofu`, `terraform`). |
| `--iac-version` | | String | Yes | Tool version (e.g. `1.12.5`). |
| `--execution-mode` | | String | No | Execution mode (`remote`, `local`). |
| `--description` | `-d` | String | No | Workspace description. |
| `--project` | | String | No | Project ID or name to assign workspace to. |

## Examples

```bash
terrakube workspace create -o "$ORG_ID" \
  --name "networking-prod" \
  --source "https://github.com/terrakube-io/terrakube-docker-compose" \
  --branch "main" \
  --folder "/" \
  --iac-type "tofu" \
  --iac-version "1.12.5" \
  --execution-mode "remote" \
  --output json
```
