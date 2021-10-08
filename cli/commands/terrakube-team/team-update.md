# team update

Usage

```text
 terrakube team update [flags]
```

#### Flags

```text
  -h, --help                     help for update
      --id string                Id of the Team (required)
      --manage-module            Manage Module Permissions
      --manage-provider          Manage Provider Permissions
      --manage-workspace         Manage Workspaces Permissions
  -n, --name string              Name of the Team
      --organization-id string   Organization Id (required)
```

#### Examples

  
Update the permissions for a Team 

```text
terrakube team update --organization-id e5ad0642-f9b3-48b3-9bf4-35997febe1fb --id 38b6635a-d38e-46f2-a95e-d00a416de4fd --manage-workspace=true --manage-module=false --manage-provider=true 
```

