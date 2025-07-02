# module create

Create a module

#### Usage

```
 terrakube module create [flags]
```

#### Flags

```
  -d, --description string       Description of the new module
  -h, --help                     help for create
  -n, --name string              Name of the new module (required)
      --organization-id string   Organization Id (required)
  -p, --provider string          Provider of the new module
  -s, --source string            Source of the new module
```

#### Examples

Create a new module

```
terrakube module create --organization-id e5ad0642-f9b3-48b3-9bf4-35997febe1fb -n myModule -d "module description" -p azurerm -s https://github.com/terrakube-io/terraform-sample-repository.git 
```
