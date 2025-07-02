# Terraform Execution Flow

When running a job inside the executor component the following logic is used:

* A folder to execute the job will be created using the organization id and workspace id:
  * /home/cnb/.terraform-spring-boot/executor/\{{ORGANIZATION\_ID\}}//\{{WORKSPACE\_ID\}}
* The executor component will initially clone the workspace to the following folder:
  * /home/cnb/.terraform-spring-boot/executor/\{{ORGANIZATION\_ID\}}//\{{WORKSPACE\_ID\}}/.originRepository
* All the files inside ".originalRepository" are moved to \{{ORGANIZATION\_ID\}}//\{{WORKSPACE\_ID\}} folder from previous step.
* Te executor component will create the extension folders where you can store BASH or GROOVY extensions:
  * /home/cnb/.terraform-spring-boot/executor/\{{ORGANIZATION\_ID\}}/\{{WORKSPACE\_ID\}}/.terrakube/toolsRepository
    * By default it will clone the repository "[https://github.com/AzBuilder/terrakube-extensions](https://github.com/AzBuilder/terrakube-extensions)", this can be change by using TerrakubeToolsRepository and TerrakubeToolsBranch environment variables.
  * The executor component will scan the extension folder and will add all the "sh" files to the linux path when using BASH commands.
  * The executor component will scan the extension folder and will add all the ".groovy" files as Groovy dependencies dynamical when using GROOVY commands.
    * When using GROOVY extension to download an external tools, the extension can use the following folder /home/cnb/.terraform-spring-boot/executor/\{{ORGANIZATION\_ID\}}/\{{WORKSPACE\_ID\}}/.terrakube/tools all the folder inside the "tools" folder are also included in the linux execution PATH
  * By default the executor component will also include all the "sh" from the imported workspace files
* Once the executor component has all the necesarry information it will execute the selected template flow for the job.
* When the job execution is completed the current \{{ORGANIZATION\_ID\}}/\{{WORKSPACE\_ID\}} is deleted.

{% hint style="warning" %}
An example of how to import external tools can be found in the Terratag Groovy extension

[https://github.com/AzBuilder/terrakube-extensions/tree/main/groovy/TerraTag](https://github.com/AzBuilder/terrakube-extensions/tree/main/groovy/TerraTag)

This extension download the terratag binary and expose the binary to be used inside the linux PATH when using BASH commands.
{% endhint %}

When using the following template:

```yaml
flow:
  - type: "terraformPlan"
    name: "Plan"
    step: 100
    commands:
      - runtime: "GROOVY"
        priority: 100
        before: true
        script: |
          import TerraTag
          new TerraTag().loadTool(
            "$workingDirectory",
            "$bashToolsDirectory",
            "0.1.30")
          "Terratag download completed"
      - runtime: "BASH"
        priority: 200
        before: true
        script: |
          helloWorld.sh
```

The following directory structure will be generating when using a workspace with the workspace with "[https://github.com/AzBuilder/terrakube-docker-compose](https://github.com/AzBuilder/terrakube-docker-compose)"

```
  | - .originRepository
  |  | - .git
  |  |  | - refs
  |  |  |  | - heads
  |  |  |  | - tags
  |  |  | - logs
  |  |  |  | - refs
  |  |  |  |  | - heads
  |  |  | - objects
  |  |  |  | - info
  |  |  |  | - pack
  |  |  | - branches
  |  |  | - hooks
  |  | - getting-started
  |  | - local
  | - .git
  |  | - refs
  |  |  | - heads
  |  |  | - tags
  |  | - logs
  |  |  | - refs
  |  |  |  | - heads
  |  | - objects
  |  |  | - info
  |  |  | - pack
  |  | - branches
  |  | - hooks
  | - getting-started
  | - local
  | - .terraform
  |  | - providers
  |  |  | - registry.terraform.io
  |  |  |  | - hashicorp
  |  |  |  |  | - null
  |  |  |  |  |  | - 3.2.1
  |  |  |  |  |  |  | - linux_amd64
  |  |  |  |  | - time
  |  |  |  |  |  | - 0.9.1
  |  |  |  |  |  |  | - linux_amd64
  | - .terrakube
  |  | - tools
  |  |  | - terratag
  |  | - toolsRepository
  |  |  | - .git
  |  |  |  | - refs
  |  |  |  |  | - heads
  |  |  |  |  | - tags
  |  |  |  |  | - remotes
  |  |  |  |  |  | - origin
  |  |  |  | - logs
  |  |  |  |  | - refs
  |  |  |  |  |  | - heads
  |  |  |  | - objects
  |  |  |  |  | - info
  |  |  |  |  | - pack
  |  |  |  | - branches
  |  |  |  | - hooks
  |  |  | - bash
  |  |  |  | - helloWorld
  |  |  |  | - terratag
  |  |  | - groovy
  |  |  |  | - Context
  |  |  |  | - Infracost
  |  |  |  | - Opa
  |  |  |  | - Sendgrid
  |  |  |  | - SlackApp
  |  |  |  | - Snyk
  |  |  |  | - TerraTag
  |  |  |  | - Terrascan
  |  |  | - policy
  |  |  | - templates
  |  |  |  | - terratag
```
