# Operations Command (`terrakube operations`)

Submit JSON:API atomic operations batch requests to execute multiple resource operations in a single transactional or aggregated call.

- **Alias**: `terrakube ops`

## Usage

```bash
terrakube operations --file <PATH_TO_JSON> [flags]
terrakube ops -f <PATH_TO_JSON> [flags]
```

## Flags

| Flag | Short | Type | Required | Description |
| :--- | :--- | :--- | :--- | :--- |
| `--file` | `-f` | String | Yes | Path to JSON file containing atomic operations payload. |
| `--output` | `-o` | String | No | Output format (`json`, `table`, `yaml`, `tsv`). |

## Payload Format

The file must contain an `atomic:operations` JSON array conforming to the JSON:API Atomic Operations specification:

```json
{
  "atomic:operations": [
    {
      "op": "add",
      "data": {
        "type": "organization",
        "attributes": {
          "name": "finance-division",
          "description": "Finance engineering workspaces",
          "executionMode": "remote"
        }
      }
    }
  ]
}
```

## Examples

### Submit an atomic batch file
```bash
terrakube operations -f batch.json --output json
```

### Submit using short alias
```bash
terrakube ops --file /tmp/bulk-workspaces.json --output yaml
```
