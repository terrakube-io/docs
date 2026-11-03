# `terrakube workspace get`

Retrieves workspace attributes.

## Usage

```bash
terrakube workspace get --organization <ORG> --id <WS_ID>
```

## Flags

| Flag | Short | Type | Required | Description |
| :--- | :--- | :--- | :--- | :--- |
| `--organization` | `-o` | String | Yes | Organization ID or name. |
| `--id` | | String | Yes | Workspace ID. |

## Examples

```bash
terrakube workspace get -o "$ORG_ID" --id "$WS_ID" --output json
```
