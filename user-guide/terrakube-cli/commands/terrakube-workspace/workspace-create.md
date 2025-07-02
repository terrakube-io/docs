# workspace create

Create a workspace

#### Usage

```text
 terrakube workspace create [flags]
```

#### Flags

```text
  -b, --branch string              Branch of the new workspace
  -h, --help                       help for create
  -n, --name string                Name of the new workspace (required)
      --organization-id string     Id of the organization (required)
  -s, --source string              Source of the new workspace
  -t, --terraform-version string   Terraform Version use in the new workspace
```

#### Examples

Create a new workspace

```text
terrakube workspace create --organization-id 312b4415-806b-47a9-9452-b71f0753136e -n myWorkspace -s https://github.com/terrakube-io/terraform-sample-repository.git -b master -t 0.15.0
```

