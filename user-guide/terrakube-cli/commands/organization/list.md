# `terrakube organization list`

Lists all organizations accessible to the authenticated user.

## Usage

```bash
terrakube organization list [flags]
```

## Options & Flags

| Flag | Short | Type | Description |
| :--- | :--- | :--- | :--- |
| `--filter` | | String | Filter criteria expression (e.g. `name==bats`). |
| `--output` | `-o` | String | Output format (`json`, `table`, `yaml`, `tsv`). Default: `json`. |

## Examples

### List organizations in Table format
```bash
terrakube organization list --output table
```

### Filter organization by name
```bash
terrakube organization list --filter "name==bats" --output json
```
