# organization update

Update an organization

#### Usage

```
 terrakube organization update [flags]
```

#### Flags

```
  -d, --description string   Description of the organization
  -h, --help                 help for update
      --id string            Id of the organization (required)
  -n, --name string          Name of the organization
```

#### Examples

Update the description for an existing organization

```
terrakube organization update --id 38b6635a-d38e-46f2-a95e-d00a416de4fd -d "new description" 
```
