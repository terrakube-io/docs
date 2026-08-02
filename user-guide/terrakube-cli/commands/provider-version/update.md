# `terrakube provider-version update`

Updates a provider version configuration.

## Usage

```bash
terrakube provider-version update --organization <ORG> --provider <PROV> --id <VER_ID> [flags]
```

## Examples

```bash
terrakube provider-version update -o "$ORG_ID" --provider "$PROV_ID" --id "$VER_ID" --version-number "3.0.1" --protocols "5.0" --output json
```
