# Creating Workspace (Terraform CLI)

{% hint style="info" %}
**Manage Workspaces** permission is required to perform this action, please check [team-management.md](../organizations/team-management.md "mention") for more info.
{% endhint %}

Workspace can be created using just the terraform CLI, to use this feature you need to firts login to the Terrakube API using the terraform command:

```
terraform login 8080-azbuilder-terrakube-sscrnu9jbie.ws-us99.gitpod.io
```

Once the authentication is completed we can create the workspace using the following backend configuration in our terraform code.

#### Terraform Cloud Block

```
terraform {
  cloud {
    organization = "simple"
    hostname = "8080-azbuilder-terrakube-sscrnu9jbie.ws-us99.gitpod.io"

    workspaces {
      name = "samplecloud"
    }
  }
}
```

{% hint style="warning" %}
Make sure to use an organization name that is already created in the Terrakube UI
{% endhint %}

#### Terraform Remote Backend

```
terraform {
  backend "remote" {
    hostname = "8080-azbuilder-terrakube-3xqsq270uq1.ws-us82.gitpod.io"
    organization = "simple"
    workspaces {
      name = "workspace1"
    }
  }
}
```

{% hint style="warning" %}
Using tags in the cloud block is not yet supported
{% endhint %}

After you added the backend configuration you can include all your terraform code, and to create the workspace in terrakube just run:

```
terraform init
```

Once the command is completed successfully you should be able to see the workspace inside the organizacion in the UI.

You can also execute remote operation like:

```
terraform plan
terraform apply
terraform destroy
```

To include terraform variables you use `terraform.tfvars` or `terraform.tfvars.json` you can also define the variables inside the workspace using the UI.
