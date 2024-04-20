# Azure Dynamic Provider Credentials

To use Azure Dynamic Provider Credentials to authenticate to Azure without storing any sensitive credential in a workspace please use the following:

### Register Application

We need to register a new application in Microsoft Entra like the following example:

<figure><img src="../../../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

Once we have the application we need to add a federated credential.

<figure><img src="../../../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

Select type "Other" and fill the following information:

* Issuer: [https://terrakube-api.mysuperdomain.com](https://terrakube-api.mysuperdomain.com)
* Subject: organization:MY\_ORGANIZATION\_NAME:workspace:MY\_WORKSPACE\_NAME
* Audience: api://AzureADTokenExchange

<figure><img src="../../../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

You need to grant access to your azure resources to the application, for example "Contributor" or any other role

<figure><img src="../../../.gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>

Inside our workspace you will have to add the following environment variables

* ARM\_TENANT\_ID=YOUR AZURE TENANT ID
* ARM\_SUBSCRIPTION\_ID=YOUR AZURE SUBSCRIPTION ID
* ARM\_CLIENT\_ID=YOUR AZURE APPLICATION ID
* ARM\_USE\_OIDC=true
* ENABLE\_DYNAMIC\_CREDENTIALS\_AZURE=true
* WORKLOAD\_IDENTITY\_AUDIENCE\_AZURE=api://AzureADTokenExchange

<figure><img src="../../../.gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>

When running a job Terrakube will correctly authenticate to Azure without any credentials inside the workspace

<figure><img src="../../../.gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>

Terraform example using the CLI driven workflow:

```
terraform {

  cloud {
    organization = "simple"
    hostname = "8080-azbuilder-terrakube-tr6130m2jsz.ws-us110.gitpod.io"

    workspaces {
      name = "simple"
    }
  }
}

# Configure the Microsoft Azure Provider
provider "azurerm" {
  features {}
  use_cli              = false

}

resource "azurerm_resource_group" "example" {
  name     = "example2"
  location = "East US 2"
}
```

### Running example:

```
user@pop-os:~/git/dynamic_creds$ terraform apply

Running apply in Terraform Cloud. Output will stream here. Pressing Ctrl-C
will cancel the remote apply if it's still pending. If the apply started it
will stop streaming the logs, but will not stop the apply running remotely.

Preparing the remote apply...

To view this run in a browser, visit:
https://8080-azbuilder-terrakube-tr6130m2jsz.ws-us110.gitpod.io/app/simple/simple/runs/2

Waiting for the plan to start...

***************************************
Running Terraform PLAN
***************************************

Terraform used the selected providers to generate the following execution
plan. Resource actions are indicated with the following symbols:
  + create

Terraform will perform the following actions:

  # azurerm_resource_group.example will be created
  + resource "azurerm_resource_group" "example" {
      + id       = (known after apply)
      + location = "eastus2"
      + name     = "example2"
    }

Plan: 1 to add, 0 to change, 0 to destroy.


Do you want to perform these actions in workspace "simple"?
  Terraform will perform the actions described above.
  Only 'yes' will be accepted to approve.

  Enter a value: yes

azurerm_resource_group.example: Creating...
azurerm_resource_group.example: Creation complete after 2s [id=/subscriptions/583006a4-7a57-4a3d-899c-620faa582f6d/resourceGroups/example2]

Apply complete! Resources: 1 added, 0 changed, 0 destroyed.
```
