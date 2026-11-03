# Migrating to Terrakube

if you are using other terraform remote states backend like the following example

{% tabs %}
{% tab title="remote backend" %}
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
{% endtab %}

{% tab title="cloud block" %}
```
terraform {
  cloud {
    hostname = "app.terraform.io"
    organization = "migrate-org"
    workspaces {
      name = "migrate-state"
    }
  }
}
```
{% endtab %}
{% endtabs %}

You could easily migrate your state to Terrakube changing the hostname to your Terrakube API endpointd.

{% tabs %}
{% tab title="remote backend" %}
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
{% endtab %}

{% tab title="cloud block" %}
```
terraform {
  cloud {
    hostname = "8080-azbuilder-terrakube-q8aleg88vlc.ws-us92.gitpod.io"
    organization = "migrate-org"
    workspaces {
      name = "migrate-state"
    }
  }
}
```
{% endtab %}
{% endtabs %}

And run the command

```
terraform login 8080-azbuilder-terrakube-q8aleg88vlc.ws-us92.gitpod.io
terraform init -migrate-state
```

Once the migration process is completed you should see the terraform state in your storage backend (azure, aws, gcp or minio) depending of your configuration
