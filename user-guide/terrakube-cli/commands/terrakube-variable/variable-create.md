# variable create

Create a variable

#### Usage

```
terrakube workspace variable create [flags]
```

#### Flags

```
  -c, --category string          Category of the new variable. Valid values are TERRAFORM or ENV
  -d, --description string       Description of the new variable
      --hcl                      Whether to evaluate the value of the variable as a string of HCL code. Has no effect for environment variables.
  -h, --help                     help for create
  -k, --key string               Key of the new variable (required)
      --organization-id string   Organization Id (required)
  -s, --sensitive                Whether the value is sensitive. If true then the variable is written once and not visible thereafter.
  -v, --value string             Value for the new variable
  -w, --workspace-id string      Workspace Id (required)
```

Create a new variable

```
terrakube workspace variable create --organization-id e5ad0642-f9b3-48b3-9bf4-35997febe1fb -w 38b6635a-d38e-46f2-a95e-d00a416de4fd -k tag_name -v "Hola mundo" --hcl=false --sensitive=false --category TERRAFORM 
```
