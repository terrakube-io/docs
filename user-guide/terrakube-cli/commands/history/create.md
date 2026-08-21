# `terrakube history create`

Creates a new execution history record.

## Usage

```bash
terrakube history create --organization <ORG_NAME_OR_ID> --workspace <WORKSPACE_NAME_OR_ID> [flags]
```

## Flags

| Flag | Short | Type | Required | Description |
| :--- | :--- | :--- | :--- | :--- |
| `--organization` | `-o` | String | Yes | Name or ID of the parent organization. |
| `--workspace` | `-w` | String | Yes | Name or ID of the parent workspace. |
| `--job-reference` | | String | No | Job reference ID. |
| `--output` | | String | No | Output data payload. |
| `--serial` | | Int | No | State serial sequence number. |
| `--md5` | | String | No | MD5 hash checksum. |
| `--lineage` | | String | No | Lineage identifier. |

## Examples

### Create a history entry
```bash
terrakube history create \
  -o "demo-org" \
  -w "docker-compose-infra" \
  --job-reference "12" \
  --serial 1 \
  --output json
```
