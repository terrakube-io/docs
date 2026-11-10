# `terrakube login`

Authenticates the CLI with a Terrakube Server instance and saves credentials to `~/.terrakube-cli.yaml`.

## Usage

```bash
terrakube login --api-url <URL> --token <PAT_TOKEN>
```

## Flags

| Flag | Short | Type | Required | Description |
| :--- | :--- | :--- | :--- | :--- |
| `--api-url` | `-a` | String | Yes | Base URL of your Terrakube API server (e.g., `https://terrakube-api.example.com`). |
| `--token` | `-t` | String | Yes | Personal Access Token (PAT) or Bearer Token. |

## Examples

### Authenticate to local instance
```bash
terrakube login -a "http://localhost:8080" -t "secret-token"
```

### Authenticate to production instance
```bash
terrakube login --api-url "https://terrakube.mycompany.com" --token "eyJhbGciOi..."
```
