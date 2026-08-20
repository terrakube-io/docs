# `terrakube job create`

Triggers a new execution job on a workspace (e.g. `plan`, `apply`, `destroy`).

## Usage

```bash
terrakube job create --organization <ORG_NAME_OR_ID> --workspace <WORKSPACE_NAME_OR_ID> --command <COMMAND> [flags]
```

## Flags

| Flag | Short | Type | Required | Description |
| :--- | :--- | :--- | :--- | :--- |
| `--organization` | `-o` | String | Yes | Name or ID of the parent organization. |
| `--workspace` | `-w` | String | Yes | Name or ID of the target workspace. |
| `--command` | `-c` | String | Yes | Command to execute (`plan`, `apply`, `destroy`). |
| `--commit-id` | | String | No | VCS commit SHA/ID. |
| `--override-branch` | | String | No | Override git branch. |
| `--override-source` | | String | No | Override git source repository URL. |
| `--plan-changes` | | Bool | No | Flag indicating if plan contains changes. |
| `--refresh` | | Bool | No | Refresh state during execution. |
| `--refresh-only` | | Bool | No | Refresh state only mode. |
| `--target-addrs` | | StringSlice | No | Target specific resource addresses. |
| `--replace-addrs` | | StringSlice | No | Replace specific resource addresses. |
| `--output` | | String | No | Output format (`json`, `table`, `yaml`, `tsv`). |

## Examples

### Trigger a plan job
```bash
terrakube job create \
  -o "demo-org" \
  -w "docker-compose-infra" \
  --command "plan" \
  --output json
```

### Trigger an apply job on a custom branch
```bash
terrakube job create \
  -o "demo-org" \
  -w "docker-compose-infra" \
  --command "apply" \
  --override-branch "feat/new-cluster" \
  --output table
```
