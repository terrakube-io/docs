# variable list

List variables

#### Usage

```
terrakube workspace variable list [flags]
```

#### Flags

```
  -f, --filter string            Filter
  -h, --help                     help for list
      --organization-id string   Organization Id (required)
  -w, --workspace-id string      Workspace Id (required)
```

#### Examples

List all existing variables for a workspace

```
terrakube workspace variable list --organization-id e5ad0642-f9b3-48b3-9bf4-35997febe1fb -w 38b6635a-d38e-46f2-a95e-d00a416de4fd
```
