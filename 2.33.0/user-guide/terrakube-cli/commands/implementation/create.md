# `terrakube implementation create`

Registers a platform binary build (OS/arch), SHA256 hash, and PGP key signatures for a provider version.

## Usage

```bash
terrakube implementation create --organization <ORG> --provider <PROV> --provider-version <VER> --os <OS> --arch <ARCH> --filename <FILE> --download-url <URL> --shasums-url <SUMS_URL> --shasums-signature-url <SIG_URL> --shasum <HASH> --key-id <KEY_ID> --ascii-armor <ARMOR> [flags]
```

## Flags

| Flag | Short | Type | Required | Description |
| :--- | :--- | :--- | :--- | :--- |
| `--organization` | `-o` | String | Yes | Organization ID or name. |
| `--provider` | | String | Yes | Provider ID or name. |
| `--provider-version` | | String | Yes | Provider Version ID. |
| `--os` | | String | Yes | Target operating system (e.g. `linux`, `darwin`, `windows`). |
| `--arch` | | String | Yes | Target architecture (e.g. `amd64`, `arm64`). |
| `--filename` | | String | Yes | Binary zip filename. |
| `--download-url` | | String | Yes | Binary download URL. |
| `--shasums-url` | | String | Yes | SHA256SUMS file URL. |
| `--shasums-signature-url` | | String | Yes | SHA256SUMS signature file URL. |
| `--shasum` | | String | Yes | SHA256 checksum string. |
| `--key-id` | | String | Yes | PGP GPG Key ID. |
| `--ascii-armor` | | String | Yes | PGP Public Key ASCII armor block. |
| `--trust-signature` | | String | No | Trust level string. |
| `--source` | | String | No | Provider source owner (e.g. `HashiCorp`). |
| `--source-url` | | String | No | Security source URL. |

## Examples

```bash
PGP_ARMOR="-----BEGIN PGP PUBLIC KEY BLOCK-----\n...\n-----END PGP PUBLIC KEY BLOCK-----"

terrakube implementation create -o "$ORG_ID" \
  --provider "$PROV_ID" \
  --provider-version "$VER_ID" \
  --os "linux" \
  --arch "amd64" \
  --filename "terraform-provider-random_3.0.1_linux_amd64.zip" \
  --download-url "https://releases.hashicorp.com/terraform-provider-random/3.0.1/terraform-provider-random_3.0.1_linux_amd64.zip" \
  --shasums-url "https://releases.hashicorp.com/terraform-provider-random/3.0.1/terraform-provider-random_3.0.1_SHA256SUMS" \
  --shasums-signature-url "https://releases.hashicorp.com/terraform-provider-random/3.0.1/terraform-provider-random_3.0.1_SHA256SUMS.72D7468F.sig" \
  --shasum "e385e00e7425dda9d30b74ab4ffa4636f4b8eb23918c0b763f0ffab84ece0c5c" \
  --key-id "34365D9472D7468F" \
  --ascii-armor "$PGP_ARMOR" \
  --trust-signature "5.0" \
  --source "HashiCorp" \
  --source-url "https://www.hashicorp.com/security.html" \
  --output json
```
