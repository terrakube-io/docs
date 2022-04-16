# variable delete

Delete a variable

#### Usage

```
terrakube workspace variable delete [flags]
```

#### Flags

```
  -h, --help                     help for delete
      --id string                Id of the variable (required)
      --organization-id string   Organization Id (required)
  -w, --workspace-id string      Workspace Id (required)
```

#### Examples

Delete a variable

```
terrakube workspace variable delete --organization-id e5ad0642-f9b3-48b3-9bf4-35997febe1fb  -w 38b6635a-d38e-46f2-a95e-d00a416de4fd --id 38b6635a-d38e-46f2-a95e-d00a416de4fd 
```
