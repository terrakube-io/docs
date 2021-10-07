# Getting started

`terrakube cli` is Terrakube Server on the command line. It brings organizations, workspaces and other Terrakube concepts to the terminal.

### Installation

You can find installation instructions here [Install](install.md)

### Authentication

Run [`terrakube login`](commands/azb-login.md) to authenticate with your account.  But first you need to set some environment variables.

{% hint style="info" %}
You can also pass this values using [`terrakube login`](commands/azb-login.md) however is recommended to use environment variables in the case you need to authenticate several times or if you are running an automatic script.
{% endhint %}

Open your terminal and execute the following commands in order to setup your terrakube environment and get authenticated. You can get  the server, path, scheme, tenant id and client id values during the [Terrakube server deployment](../getting-started/deployment/)

```text
export TERRAKUBE_SERVER="my-terrakube.com"
export TERRAKUBE_PATH="terrakube"
export TERRAKUBE_SCHEME="https"
export TERRAKUBE_TENANT_ID="59a1b348-548d-4360-b2e2-9d2b849527a8"
export TERRAKUBE_CLIENT_ID="7a307a36-e08b-4954-a9fb-03a67e082fc4"

terrakube login
```

You will receive a code, open the url in a web browser and provide your account details. See below code as reference

```text
To sign in, use a web browser to open the page 
https://microsoft.com/devicelogin and enter the code AAM4MVU96 to authenticate.
```

### Working with Organizations

Organizations are privately shared spaces for teams to collaborate on infrastructure. 

* You can check the organizations you have access using  [`terrakube organization` ](commands/azb-organization/)

```text
terrakube organization list
```

* If you don't have an organization created yet, you can create a new one.

```text
terrakube organization create -n MyOrganization -d "Getting started Organization" 
```

 For more commands and options in organization see the full documentation for [`terrakube organization`](commands/azb-organization/)

 





