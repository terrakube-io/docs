# team create

Create a Team

#### Usage

```text
 terrakube team create [flags]
```

#### Flags

```text
  -h, --help                     help for create
      --manage-module            Manage Module Permissions
      --manage-provider          Manage Provider Permissions
      --manage-workspace         Manage Workspaces Permissions
  -n, --name string              Name of the new Team (required)
      --organization-id string   Organization Id (required)
```

#### Examples

Create a new team

```text
 terrakube team create --organization-id e5ad0642-f9b3-48b3-9bf4-35997febe1fb -n AZB_USER --manage-workspace=true --manage-module=true --manage-provider=true
```

