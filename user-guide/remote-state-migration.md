# Remote State Migration

if you are using other terraform remote states backend like the following example

```
terraform {
  backend "remote" {
    hostname = "app.terraform.io"
    organization = "migrate-org"

    workspaces {
      name = "migrate-state"
    }
  }
}
```

You could easily migrate your state to Terrakube changing the hostname to your Terrakube API endpointd.

```
terraform {
  backend "remote" {
    hostname = "8080-azbuilder-terrakube-q8aleg88vlc.ws-us92.gitpod.io"
    organization = "migrate-org"

    workspaces {
      name = "migrate-state"
    }
  }
}
```

And run the command

```
terraform login 8080-azbuilder-terrakube-q8aleg88vlc.ws-us92.gitpod.io
terraform init -migrate-state
```

Once the migration process is completed you should see the terraform state in your storage backend (azure, aws, gcp or minio) depending of your configuration

{% hint style="info" %}
This feature is supported from Terrakube 2.13.0
{% endhint %}
