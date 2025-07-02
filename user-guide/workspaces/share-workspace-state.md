# Share Workspace State

Terrakube support sharing the terraform state between workspaces using the following data block.

```
data "terraform_remote_state" "remote_creation_time" {
  backend = "remote"
  config = {
    organization = "simple"
    hostname = "8080-terrakube-io-terrakube-vg8s9w8fhaj.ws-us102.gitpod.io"
    workspaces = {
      name = "simple_tag1"
    }
  }
}
```

For example it can be used in the following way:

```
data "terraform_remote_state" "remote_creation_time" {
  backend = "remote"
  config = {
    organization = "simple"
    hostname = "8080-terrakube-io-terrakube-vg8s9w8fhaj.ws-us102.gitpod.io"
    workspaces = {
      name = "simple_tag1"
    }
  }
}

resource "null_resource" "previous" {}

resource "time_sleep" "wait_30_seconds" {
  depends_on = [null_resource.previous]

  create_duration = data.terraform_remote_state.remote_creation_time.outputs.creation_time
}

resource "null_resource" "next" {
  depends_on = [time_sleep.wait_30_seconds]
}
```

{% hint style="info" %}
This feature is supported from Terrakube 2.15.0
{% endhint %}
