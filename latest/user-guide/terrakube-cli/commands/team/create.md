# `terrakube team create`

Creates a team in an organization and grants specific permissions.

## Usage

```bash
terrakube team create --organization <ORG> --name <NAME> --role <ROLE> [flags]
```

## Flags

| Flag | Short | Type | Required | Description |
| :--- | :--- | :--- | :--- | :--- |
| `--organization` | `-o` | String | Yes | Parent Organization ID or name. |
| `--name` | `-n` | String | Yes | Team name (e.g. `TERRAKUBE_ADMIN`). |
| `--role` | | String | No | Role name (e.g. `Admin`, `User`). |
| `--manage-workspace` | | Bool | No | Permission to manage workspaces. |
| `--manage-module` | | Bool | No | Permission to manage modules. |
| `--manage-provider` | | Bool | No | Permission to manage providers. |
| `--manage-state` | | Bool | No | Permission to manage state files. |
| `--manage-collection` | | Bool | No | Permission to manage collections. |
| `--manage-vcs` | | Bool | No | Permission to manage VCS connections. |
| `--manage-template` | | Bool | No | Permission to manage workflow templates. |
| `--manage-job` | | Bool | No | Permission to trigger and cancel jobs. |
| `--plan-job` | | Bool | No | Permission to run speculative plan jobs. |
| `--approve-job` | | Bool | No | Permission to approve apply jobs. |

## Examples

```bash
terrakube team create -o "$ORG_ID" \
  --name "TERRAKUBE_ADMIN" \
  --role "Admin" \
  --manage-workspace \
  --manage-module \
  --manage-provider \
  --manage-state \
  --manage-collection \
  --manage-vcs \
  --manage-template \
  --manage-job \
  --plan-job \
  --approve-job \
  --output json
```
