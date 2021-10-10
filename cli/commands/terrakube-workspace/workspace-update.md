# workspace update

Update a workspace

#### Usage

```
 terrakube workspace update [flags]
```

#### Flags

```
  -b, --branch string              Branch of the workspace
  -h, --help                       help for update
      --id string                  Id of the workspace (required)
  -n, --name string                Name of the workspace (required)
      --organization-id string     Id of the organization (required)
  -s, --source string              Source of the workspace
  -t, --terraform-version string   Terraform Version use in the workspace
```

#### Examples

Update Terraform version in workspace

```
terrakube workspace update --organization-id 312b4415-806b-47a9-9452-b71f0753136e --id 38b6635a-d38e-46f2-a95e-d00a416de4fd -t 0.14.0
```
