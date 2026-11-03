# module update

Update a module

#### Usage

```
 terrakube module update [flags]
```

#### Flags

```
  -d, --description string       Description of the module
  -h, --help                     help for update
      --id string                Id of the module (required)
  -n, --name string              Name of the module
      --organization-id string   Organization Id (required)
  -p, --provider string          Provider of the module
  -s, --source string            Source of the module
```

Update the description of the module

```
terrakube module update --organization-id e5ad0642-f9b3-48b3-9bf4-35997febe1fb --id 38b6635a-d38e-46f2-a95e-d00a416de4fd -d "new description"
```
