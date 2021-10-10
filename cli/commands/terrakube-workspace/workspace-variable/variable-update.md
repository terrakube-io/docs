# variable update

Update a variable

#### Usage

```
terrakube workspace variable update [flags]
```

#### Flags

```
  -c, --category string          Category of the variable. Valid values are TERRAFORM or ENV
  -d, --description string       Description of the variable
      --hcl                      Whether to evaluate the value of the variable as a string of HCL code. Has no effect for environment variables.
  -h, --help                     help for update
      --id string                Id of the variable (required)
  -k, --key string               Key of the variable
      --organization-id string   Organization Id (required)
  -s, --sensitive                Whether the value is sensitive. If true then the variable is written once and not visible thereafter.
  -v, --value string             Value of the variable
  -w, --workspace-id string      Workspace Id (required)
```

#### Examples

Update the value of the variable 

```
terrakube workspace variable update --organization-id e5ad0642-f9b3-48b3-9bf4-35997febe1fb -w 38b6635a-d38e-46f2-a95e-d00a416de4fd --id 38b6635a-d38e-46f2-a95e-d00a416de4fd -v "new value" 
```
