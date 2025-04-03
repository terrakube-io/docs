# terrakube login

Authenticate with a Terrakube Server using a Personal Access Token (PAT)

#### Usage

```
terrakube login [flags]
```

#### Flags

```bash
  -h, --help               help for login
      --api-url string     Terrakube Server API URL (required)
  -t, --pat string         Personal Access Token (PAT) for authentication (required)
```

#### Examples

Login to a local Terrakube Server with a PAT token

```
terrakube login --api-url http://localhost:8080 --pat eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
```

Login to a remote Terrakube Server with a PAT token

```
terrakube login --api-url https://terrakube.example.com --pat eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
``` 
