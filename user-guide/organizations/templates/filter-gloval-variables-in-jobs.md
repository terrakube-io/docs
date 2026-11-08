# Filter gloval variables in jobs

In some special cases it is necesarry to filter the global variables used inside the job execution, if the "inpursEnv" or "inpustTerraform" is not defined all global variables will be imported to the job context.

### Template Configuration

```
flow:
  - type: "customScripts"
    step: 100
    inputsEnv:
      INPUT_IMPORT1: $IMPORT1
      INPUT_IMPORT2: $IMPORT2
    commands:
      - runtime: "BASH"
        priority: 100
        before: true
        script: |
          echo $INPUT_IMPORT1
          echo $INPUT_IMPORT2
```

> $IMPORT1 and $IMPORT2 will be added to the job context as env variable INPUT\_IMPORT1 and INPUT\_IMPORT2

### Global Variables Setup

<figure><img src="https://user-images.githubusercontent.com/4461895/225455685-4b060f97-e802-44df-97c2-dd5c2be06b7a.png" alt=""><figcaption></figcaption></figure>

### Running Workspace

<figure><img src="https://user-images.githubusercontent.com/4461895/225455661-d6b0143b-c758-4ea4-b511-e943183aeaba.png" alt=""><figcaption></figcaption></figure>

### Other configuration Options.

#### Import terraform variables

```
flow:
  - type: "customScripts"
    step: 100
    inputsTerraform:
      INPUT_IMPORT1: $IMPORT1
      INPUT_IMPORT2: $IMPORT2
    commands:
      ....
```

#### Import when importing the template

```
flow:
  - type: "terraformPlan"
    step: 100
    importComands:
      repository: "https://github.com/terrakube-io/terrakube-extensions"
      folder: "templates/terratag"
      branch: "import-template"
      inputsEnv:
        INPUT_IMPORT1: $IMPORT1
        INPUT_IMPORT2: $IMPORT2
      inputsTerraform:
        INPUT_IMPORT1: $IMPORT1
        INPUT_IMPORT2: $IMPORT2
  - type: "terraformApply"
    step: 200
```

### Import Precedence

workspace env variables > importComands env variables > Flow env variables\
workspace terraform variables > importComands terraform variables > Flow terraform variables
