# GCP Dynamic Provider Credentials

### Requirements

The dynamic provider credential setup in GCP  can be done with the Terrraform code available in the following link:

[https://github.com/AzBuilder/terrakube/tree/main/dynamic-credential-setup/gcp](https://github.com/AzBuilder/terrakube/tree/main/dynamic-credential-setup/gcp)

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
terrakube_token = "TERRAKUBE_PERSONAL_ACCESS_TOKEN"
terrakube_hostname = "terrakube-api.mydomain.com"
terrakube_organization_name = "simple"
terrakube_workspace_name = "dynamic-workspace"
gcp_project_id = "my-gcp-project"

```

{% hint style="info" %}
To generate the API token check [here](https://docs.terrakube.io/user-guide/organizations/api-tokens)
{% endhint %}

Run Terraform apply to create all the federated credential setup in GCP and a sample workspace in terrakube for testing

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

provider "google" {
  project     = "my-gcp-project"
  region      = "us-central1"
  zone        = "us-central1-c"
}

resource "google_storage_bucket" "auto-expire" {
  name          = "mysuperbukcetname"
  location      = "US"
  force_destroy = true

  public_access_prevention = "enforced"
}
```

### Running Example

<figure><img src="../../../.gitbook/assets/image (385).png" alt=""><figcaption></figcaption></figure>
