# job create

Create a job

#### Usage

```
 terrakube job create [flags]
```

#### Flags

```
  -c, --command string           Command to execute: plan,apply,destroy (required)
  -h, --help                     help for create
      --organization-id string   Organization Id (required)
  -w, --workspace-id string      Workspace Id (required)
```

#### Examples

Create a new job

```
terrakube job create --organization-id e5ad0642-f9b3-48b3-9bf4-35997febe1fb -w e5ad0642-f9b3-48b3-9bf4-35997febe1fb  -c apply
```
