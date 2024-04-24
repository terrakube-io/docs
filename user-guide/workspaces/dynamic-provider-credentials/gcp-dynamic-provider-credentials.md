# GCP Dynamic Provider Credentials

To use GCP Dynamic Provider Credentials to authenticate to GCP without storing any sensitive credential in a workspace please use the following:

### Workload Identity Federation

To create an identity federation open the you GCP project and create a new identity pool

<figure><img src="../../../.gitbook/assets/image (380).png" alt=""><figcaption></figcaption></figure>

Select OIDC , add a provider name, use the Terrakube API for the issuer URL and leave the default audience

<figure><img src="../../../.gitbook/assets/image (381).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
You can copy the audience value without the "https", this value will be require in your Terrakube workspace.
{% endhint %}

Setup the provider attributes, the mapping should look like the following:

OIDC 1:

```
assertion.sub
```

Condition CEL

```
assertion.sub.startsWith("organization:TERRAKUBE_ORGANIZATION_NAME:workspace:TERRAKUBE_WORKSPACE_NAME")
```

<figure><img src="../../../.gitbook/assets/image (382).png" alt=""><figcaption></figcaption></figure>

Once the identity provider is created we need to grant access to one particular service account that has the require access permission for our Terraform deployments

<figure><img src="../../../.gitbook/assets/image (383).png" alt=""><figcaption></figcaption></figure>

### Terrakube Workspace Setup

To enable gcp dynamic credentials in our workspace inside Terrakube we need to add the following environment variables:

* ENABLE\_DYNAMIC\_CREDENTIALS\_GCP=true
* WORKLOAD\_IDENTITY\_SERVICE\_ACCOUNT\_EMAIL=[xxxx@xxxx.iam.gserviceaccount.com](mailto:xxxx@xxxx.iam.gserviceaccount.com)
* WORKLOAD\_IDENTITY\_AUDIENCE\_GCP=//iam.googleapis.com/projects/\{{PROJECT-NUMBER\}}/locations/global/workloadIdentityPools/\{{PROJECT\_NAME\}}/providers/\{{PROVIDER\}}

{% hint style="info" %}
Make sure there are no GOOGLE\_CREDENTIALS or GOOGLE\_APPLICATION\_CREDENTIALS in your workspace configuration
{% endhint %}

Your workspace should look like the following:

<figure><img src="../../../.gitbook/assets/image (384).png" alt=""><figcaption></figcaption></figure>

When you workspace job is running Terrakube will autenthicate to your GCP project automatically without using any kind of credential like the following example:



```
terraform {

  cloud {
    organization = "simple"
    hostname = "8080-azbuilder-terrakube-2vs2w68kc0p.ws-us110.gitpod.io"

    workspaces {
      name = "simple"
    }
  }
}

provider "google" {
  project     = "XXXXXXX"
  region      = "us-central1"
  zone        = "us-central1-c"
}

resource "google_storage_bucket" "auto-expire" {
  name          = "XXXXXXXXX"
  location      = "US"
  force_destroy = true

  public_access_prevention = "enforced"
}
```

<figure><img src="../../../.gitbook/assets/image (385).png" alt=""><figcaption></figcaption></figure>
