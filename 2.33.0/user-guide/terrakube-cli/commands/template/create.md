# `terrakube template create`

Creates a new workflow template with base64-encoded TCL YAML content.

## Usage

```bash
terrakube template create --organization <ORG> --name <NAME> --description <DESC> --version <VER> --content <BASE64_YAML> [flags]
```

## Flags

| Flag | Short | Type | Required | Description |
| :--- | :--- | :--- | :--- | :--- |
| `--organization` | `-o` | String | Yes | Organization ID or name. |
| `--name` | `-n` | String | Yes | Template name. |
| `--description` | `-d` | String | Yes | Description. |
| `--version` | | String | Yes | Version semantic string (e.g. `1.0.0`). |
| `--content` | | String | Yes | Base64-encoded string of TCL YAML workflow definition. |

## Examples

```bash
TCL_RAW="flow:
  - type: \"terraformPlan\"
    step: 100
  - type: \"terraformApply\"
    step: 200"
TCL_B64=$(echo "$TCL_RAW" | base64 -w 0)

terrakube template create -o "$ORG_ID" \
  --name "custom-plan-apply" \
  --description "Standard plan and apply flow" \
  --version "1.0.0" \
  --content "$TCL_B64" \
  --output json
```
