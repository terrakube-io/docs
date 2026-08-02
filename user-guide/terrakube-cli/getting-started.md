# Getting Started

The Terrakube CLI (`terrakube`) is the command-line interface for managing Terrakube Server resources. This guide walks you through authenticating, configuring environment defaults, and running an end-to-end management script.

---

## 1. Authentication (`login` & `logout`)

To interact with a Terrakube instance, you must authenticate using your Terrakube Server API URL and a Personal Access Token (PAT).

### Login

Authenticate and persist your credentials locally:

```bash
terrakube login --api-url "https://terrakube-api.example.com" --token "YOUR_PERSONAL_ACCESS_TOKEN"
```

Short flags can also be used:
```bash
terrakube login -a "https://terrakube-api.example.com" -t "YOUR_PERSONAL_ACCESS_TOKEN"
```

This command validates your credentials and writes configuration settings to `~/.terrakube-cli.yaml`.

### Logout

To clear saved credentials from your machine:

```bash
terrakube logout
```

---

## 2. Configuration & Environment Variables

`terrakube` resolves parameters using the following order of precedence:
1. **Command-line flags** (e.g. `--organization`, `--output`)
2. **Environment variables**
3. **Configuration file** (`~/.terrakube-cli.yaml`)

### Environment Variables

| Variable | Description |
| :--- | :--- |
| `TERRAKUBE_API_URL` | Base API URL of your Terrakube instance (e.g., `http://localhost:8080`) |
| `TERRAKUBE_TOKEN` | Bearer token / Personal Access Token |
| `TERRAKUBE_ORGANIZATION` | Default Organization ID or Name |
| `TERRAKUBE_CONFIG` | Custom path to config file |

Example environment configuration:
```bash
export TERRAKUBE_API_URL="http://localhost:8080"
export TERRAKUBE_TOKEN="secret-pat-token"
export TERRAKUBE_ORGANIZATION="my-org"
```

---

## 3. Global Command Options

All `terrakube` subcommands support the following global options:

- `--output`, `-o`: Sets output format (`json`, `table`, `yaml`, `tsv`, `none`). Default: `json`.
- `--hide-nulls`: Hides null fields in JSON/YAML output (boolean). Default: `true`.
- `--organization`, `-o`: Default organization ID or name for org-scoped resources.
- `--config`: Path to configuration file. Default: `~/.terrakube-cli.yaml`.

---

## 4. End-to-End Quickstart Script

Below is a complete Bash script demonstrating how to log in, create an organization, set up an admin team, launch a workspace with OpenTofu, configure workspace environment variables, create a project, and assign the workspace to the project.

```bash
#!/usr/bin/env bash
set -e

# Configurable parameters
TERRAKUBE_API_URL="${TERRAKUBE_API_URL:-http://localhost:8080}"
TERRAKUBE_PAT="${TERRAKUBE_PAT:-your-pat-token}"

# Generate random 4-character alphanumeric suffix
RAND_SUFFIX=$(head /dev/urandom | tr -dc 'a-z0-9' | head -c 4)
ORG_NAME="production${RAND_SUFFIX}"
WS_NAME="networking${RAND_SUFFIX}"
PRJ_NAME="core${RAND_SUFFIX}"

echo "==> 1. Authenticating with Terrakube Server..."
terrakube login -a "$TERRAKUBE_API_URL" -t "$TERRAKUBE_PAT"

echo "==> 2. Creating Organization '$ORG_NAME'..."
ORG_JSON=$(terrakube organization create \
  --name "$ORG_NAME" \
  --description "Production Workloads" \
  --execution-mode "remote" \
  --output json)
ORG_ID=$(echo "$ORG_JSON" | jq -r '.id')
echo "Organization created with ID: $ORG_ID"

echo "==> 3. Creating Admin Team 'TERRAKUBE_ADMIN'..."
TEAM_JSON=$(terrakube team create \
  -o "$ORG_ID" \
  --name "TERRAKUBE_ADMIN" \
  --role "Admin" \
  --manage-workspace \
  --manage-module \
  --manage-provider \
  --manage-state \
  --manage-template \
  --manage-job \
  --output json)
TEAM_ID=$(echo "$TEAM_JSON" | jq -r '.id')
echo "Team created with ID: $TEAM_ID"

echo "==> 4. Creating Workspace '$WS_NAME'..."
WS_JSON=$(terrakube workspace create \
  -o "$ORG_ID" \
  --name "$WS_NAME" \
  --description "Production Virtual Networks" \
  --source "https://github.com/terrakube-io/terrakube-docker-compose" \
  --branch "main" \
  --folder "/" \
  --iac-type "tofu" \
  --iac-version "1.12.5" \
  --execution-mode "remote" \
  --output json)
WS_ID=$(echo "$WS_JSON" | jq -r '.id')
echo "Workspace created with ID: $WS_ID"

echo "==> 5. Adding Workspace Variables..."
terrakube variable create \
  -o "$ORG_ID" \
  -w "$WS_ID" \
  --key "REGION" \
  --value "us-east-1" \
  --category "ENV"

terrakube variable create \
  -o "$ORG_ID" \
  -w "$WS_ID" \
  --key "ENVIRONMENT" \
  --value "production" \
  --category "ENV"

echo "==> 6. Creating Project '$PRJ_NAME'..."
PRJ_JSON=$(terrakube project create \
  -o "$ORG_ID" \
  --name "$PRJ_NAME" \
  --description "Core cloud setup" \
  --output json)
PRJ_ID=$(echo "$PRJ_JSON" | jq -r '.id')
echo "Project created with ID: $PRJ_ID"

echo "==> 7. Assigning Workspace to Project..."
terrakube workspace update \
  -o "$ORG_ID" \
  --id "$WS_ID" \
  --name "$WS_NAME" \
  --project "$PRJ_ID" \
  --source "https://github.com/terrakube-io/terrakube-docker-compose" \
  --branch "main" \
  --folder "/" \
  --iac-type "tofu" \
  --iac-version "1.12.5" \
  --execution-mode "remote"

echo "==> Quickstart workflow completed successfully!"
```
