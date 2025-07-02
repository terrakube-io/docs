# Azure Dynamic Provider Credentials

### Requirements

The dynamic provider credential setup in Azure  can be done with the Terrraform code available in the following link:

[https://github.com/AzBuilder/terrakube/tree/main/dynamic-credential-setup/azure](https://github.com/AzBuilder/terrakube/tree/main/dynamic-credential-setup/azure)

{% hint style="warning" %}
The code will also create a sample workspace with all the require environment variables that can be used to test the functionality using the CLI driven workflow.
{% endhint %}

Make sure to mount your public and private key to the API container as explained [here](https://docs.terrakube.io/user-guide/workspaces/dynamic-provider-credentials#generate-public-and-private-key)

{% hint style="info" %}
Mare sure the private key is in _**"pkcs8"**_ format
{% endhint %}

Validate the following terrakube api endpoints are working:

* [https://terrakube-api.mydomain.com/.well-known/jwks](https://terrakube-api.mydomain.com/.well-known/jwks)
* [https://terrakube-api.mydomain.com/.well-known/openid-configuration](https://terrakube-api.mydomain.com/.well-known/openid-configuration)

Set terraform variables using: _**"variables.auto.tfvars"**_

```
terrakube_token                          = "TERRAKUBE_PERSONAL_ACCESS_TOKEN"
terrakube_api_hostname                   = "TERRAKUBE-API.MYCLUSTER.COM"
terrakube_federated_credentials_audience = "api://AzureADTokenExchange"
terrakube_organization_name              = "simple"
terrakube_workspace_name                 = "dynamic-azure"
```

{% hint style="info" %}
To generate the API token check [here](https://docs.terrakube.io/user-guide/organizations/api-tokens)
{% endhint %}

Run Terraform apply to create all the federated credential setup in AWS  and a sample workspace in terrakube for testing

To test the following terraform code can be used:

```
terraform {

  cloud {
    organization = "terrakube_organization_name"
    hostname = "terrakube-api.mydomain.com"

    workspaces {
      name = "terrakube_workspace_name"
    }
  }
}

provider "azurerm" {
  features {}
}

 resource "azurerm_resource_group" "example" {
  name     = "randomstring-aejthtyu"
  location = "East US 2"
}


```

### Running Example:

<figure><img src="../../../.gitbook/assets/image (60).png" alt=""><figcaption></figcaption></figure>

When running a job Terrakube will correctly authenticate to Azure without any credentials inside the workspace

<figure><img src="../../../.gitbook/assets/image (61).png" alt=""><figcaption></figcaption></figure>
