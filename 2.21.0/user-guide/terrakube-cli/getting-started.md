# Getting started

`terrakube cli` is Terrakube on the command line. It brings organizations, workspaces and other Terrakube concepts to the terminal.

### Installation

You can find installation instructions here [Install](install.md)

### Getting help

When you are working with terrakube cli probably you will need some help with the supported commands and flags and you can use [Commands sections](commands/) for that, however a better option is to use the same cli to get help using --help flag or -h shortand. The embedded help provides you more detail for each command and some examples you can use as reference

```bash
# using help flag
terrakube --help

# using shorthand
terrakube -h

# getting help for an specific command
terrakube organization -h

# getting help for an specific sub command
terrakube organization create -h
```

### Authentication

Run [`terrakube login`](commands/azb-login.md) to authenticate with your account. But first you need to set some environment variables.

{% hint style="info" %}
You can also pass this values using [`terrakube login`](commands/azb-login.md) however is recommended to use environment variables in the case you need to authenticate several times or if you are running an automatic script.
{% endhint %}

Open your terminal and execute the following commands in order to setup your terrakube environment and get authenticated. You can get the server, path, scheme, tenant id and client id values during the [Terrakube server deployment](../../getting-started/deployment/)

```
export TERRAKUBE_SERVER="my-terrakube.com"
export TERRAKUBE_PATH="terrakube"
export TERRAKUBE_SCHEME="https"
export TERRAKUBE_TENANT_ID="59a1b348-548d-4360-b2e2-9d2b849527a8"
export TERRAKUBE_CLIENT_ID="7a307a36-e08b-4954-a9fb-03a67e082fc4"

terrakube login
```

You will receive a code, open the url in a web browser and provide your account details. See below code as reference

```
To sign in, use a web browser to open the page 
https://microsoft.com/devicelogin and enter the code AAM4MVU96 to authenticate.
```

### Create your Organization

Organizations are privately shared spaces for teams to collaborate on infrastructure.

* You can check the organizations you have access using [`terrakube organization`](commands/azb-organization/)

```
terrakube organization list
```

* If you don't have an organization created yet, you can create a new one.

```
terrakube organization create --name MyOrganization --description "Getting started Organization" 
```

The result for the above command should be something like this

```
{
    "attributes": {
        "description": "Getting started Organization",
        "name": "MyOrganization"
    },
    "id": "8a6e9998-165c-49f0-953c-d3fb0924731a",
    "relationships": {
        "job": {},
        "module": {},
        "workspace": {}
    },
    "type": "organization"
}
```

For more commands and options in organization see the full documentation for [`terrakube organization`](commands/azb-organization/)

### Working with Teams

Once you create your organization you will probably want to define the teams and permissions for the organization, so you can use the team command for that.

```
terrakube team create --organization-id 8a6e9998-165c-49f0-953c-d3fb0924731a --name AZB_USER --manage-workspace=true --manage-module=true --manage-provider=true
```

In the previous command we are creating a new team inside our new Organization and for this case we are providing permissions to manage workspaces, modules and providers. In the name flag we are using an Azure AD Group, so all the team members are defined inside the AD group. For more details about teams and permissions you can see [Security Page](../../getting-started/security.md).

### Create a Workspace

After having given permissions to our teams we can create a workspace.

```
terrakube workspace create --organization-id 8a6e9998-165c-49f0-953c-d3fb0924731a --name MyWorkspace --source https://github.com/AzBuilder/terraform-sample-repository.git --branch master --terraform-version 0.15.0
```

And define some variables for the created workspace

```
terrakube workspace variable create --organization-id 8a6e9998-165c-49f0-953c-d3fb0924731a --workspace-id 38b6635a-d38e-46f2-a95e-d00a416de4fd --key tag_name --value "Hola mundo" --hcl=false --sensitive=false --category TERRAFORM 
```

If you want to avoid entering the organization or the workspace in each command you can use environment variables, so the previous commands would be simplified as follows.

```
export TERRAKUBE_ORGANIZATION_ID=8a6e9998-165c-49f0-953c-d3fb0924731a
terrakube workspace create --name MyWorkspace --source https://github.com/AzBuilder/terraform-sample-repository.git --branch master --terraform-version 0.15.0
export TERRAKUBE_WORKSPACE_ID=38b6635a-d38e-46f2-a95e-d00a416de4fd
terrakube workspace variable create --key tag_name --value "Hola mundo" --hcl=false --sensitive=false --category TERRAFORM 
```

### Run a Job

Now that you have a workspace with all the variables defined, you can execute a job, basically a job is a remote terraform apply, plan or destroy. So if you want to run apply we can execute the following command.

```bash
terrakube job create --organization-id 8a6e9998-165c-49f0-953c-d3fb0924731a --workspace-id 38b6635a-d38e-46f2-a95e-d00a416de4fd  --command apply 
```

After a few minutes the job will finish and you can check the result

```bash
terrakube job list --organization-id 8a6e9998-165c-49f0-953c-d3fb0924731a
```

{% hint style="info" %}
You can use a curl command to retrieve the log result from the terminal
{% endhint %}

### Defining your Modules

Usually you will want to define your infrastructure templates as code using terraform and for this you can use the modules so others can reuse them.

```bash
terrakube module create --organization-id 8a6e9998-165c-49f0-953c-d3fb0924731a --name myModule --description "module description" --provider azurerm --source https://github.com/AzBuilder/terraform-sample-repository.git 
```

### Simplifying the commands

In order to simplify the commands when you are working with the cli, you can use shortands and alias and some environment variables. See the following sections for more details.

{% hint style="info" %}
Use the --help flag to get the details about available shorthands and alias for each command
{% endhint %}

### Using shorthands

```bash
# without shorthand
terrakube organization create --name MyOrganization --description "Getting started Organization" 

# using shorthand
terrakube organization create -n MyOrganization -d "Getting started Organization" 
```

### Using alias

```bash
# without alias
terrakube organization create --name MyOrganization --description "Getting started Organization"

# using alias and shorthand
terrakube org create -n MyOrganization -d "Getting started Organization"
```

### Using environment variables

```bash
# creating multiple modules without env variables
terrakube module create --organization-id 8a6e9998-165c-49f0-953c-d3fb0924731a --name myModule --description "module description" --provider azurerm --source https://github.com/AzBuilder/terraform-sample-repository.git 
terrakube module create --organization-id 8a6e9998-165c-49f0-953c-d3fb0924731a --name myModule2 --description "module description 2" --provider azurerm --source https://github.com/AzBuilder/terraform-sample-repository.git
terrakube module create --organization-id 8a6e9998-165c-49f0-953c-d3fb0924731a --name myModule3 --description "module description 3" --provider azurerm --source https://github.com/AzBuilder/terraform-sample-repository.git

# creating multiple modules using shorthand, alias and env variables
export TERRAKUBE_ORGANIZATION_ID=8a6e9998-165c-49f0-953c-d3fb0924731a
terrakube mod create -n myModule -d "module description" -p azurerm -s https://github.com/AzBuilder/terraform-sample-repository.git 
terrakube mod create -n myModule2 -d "module description 2" -p azurerm -s https://github.com/AzBuilder/terraform-sample-repository.git
terrakube mod create -n myModule3 -d "module description 3" -p azurerm -s https://github.com/AzBuilder/terraform-sample-repository.git
```
