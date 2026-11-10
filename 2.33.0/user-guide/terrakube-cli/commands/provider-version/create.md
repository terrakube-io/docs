# `terrakube provider-version create`

Registers a new version for a private provider.

## Usage

```bash
terrakube provider-version create --organization <ORG> --provider <PROV> --version-number <VER> --protocols <PROTOCOLS>
```

## Flags

| Flag | Short | Type | Required | Description |
| :--- | :--- | :--- | :--- | :--- |
| `--organization` | `-o` | String | Yes | Organization ID or name. |
| `--provider` | | String | Yes | Provider ID or name. |
| `--version-number` | | String | Yes | Version semantic string (e.g. `3.0.1`). |
| `--protocols` | | String | Yes | Supported protocol versions (e.g. `5.0`). |

## Examples

```bash
terrakube provider-version create -o "$ORG_ID" \
  --provider "$PROV_ID" \
  --version-number "3.0.1" \
  --protocols "5.0" \
  --output json
```
