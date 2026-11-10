# terrakube login

Authenticate with a Terrakube Server

#### Usage

```
terrakube login [flags]
```

#### Flags

```bash
  -c, --client-id string   Azure application Client id (required)
  -h, --help               help for login
  -p, --path string        Terrakube Server path
      --scheme string      Terrakube Server scheme: http or https (default "http")
  -s, --server string      Terrakube Server url (required)
  -t, --tenant-id string   Azure tenant Id (required)
```

#### Examples

Login to a local Terrakube Server

```
terrakube login -s localhost:8080 -c 853b26d6-1849-4c00-8543-da5805b0e593 -t 0e6427af-ab9e-4af6-9f6f-bc098f470d75
```
