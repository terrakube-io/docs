# CLI-driven Workflow

With the CLI integration, you can use Terrakube’s collaboration features in the Terraform CLI workflow that you are already familiar with. This gives you the best of both worlds as a developer who uses the Terraform CLI, and it also works well with your existing CI/CD pipelines.

You can initiate runs with the usual terraform plan and terraform apply commands and then monitor the run progress from your terminal. These runs run remotely in Terrakube.

{% hint style="info" %}
Terrakube creates default templates for CLI-Driven workflow when you create a new organization, if you delete those templates you won't be able to run this workflow, check [default templates](../organizations/templates/default-templates.md#cli-driven-templates) for more information.
{% endhint %}

{% hint style="warning" %}
&#x20;At the moment all the terraform operations are executed remotely in Terrakube
{% endhint %}

### Configuration

To use the CLI-driven workflow the firts step will be to setup our project to use the terraform remote backend or the `cloud` block as the following example:

{% tabs %}
{% tab title="remote backend" %}
```
terraform {
  backend "remote" {
    hostname = "terrakube-api.example.com"
    organization = "simple"
    workspaces {
      name = "workspace1"
    }
  }
}

resource "random_string" "random" {
  length           = 16
  special          = true
  override_special = "/@£$"
}
```
{% endtab %}

{% tab title="cloud block" %}
```
terraform {
  cloud {
    organization = "simple"
    hostname = "terrakube-api.example.com"
    workspaces {
      name = "samplecloud"
    }
  }
}

resource "random_string" "random" {
  length           = 16
  special          = true
  override_special = "/@£$"
}
```
{% endtab %}

{% tab title="cloud block tags" %}
```
terraform {
  cloud {
    organization = "simple"
    hostname = "terrakube-api.example.com"

    workspaces {
      tags = ["development", "networking"]
    }
  }
}

resource "random_string" "random" {
  length           = 16
  special          = true
  override_special = "/@£$"
}
```
{% endtab %}

{% tab title="cloud block key/value tags" %}
```
terraform {
  cloud {
    organization = "simple"
    hostname = "terrakube-api.example.com"

    workspaces {
      tags = {
        env = "dev"
        app = "networking"
      }
    }
  }
}

resource "random_string" "random" {
  length           = 16
  special          = true
  override_special = "/@£$"
}
```
{% endtab %}
{% endtabs %}

* Hostname
  * This should be the Terrakube API
* Organization
  * This should be the Terrakube organization where the workspace will be created.&#x20;
* Workspace
  * The name of the workspace to be created. Or you can use an existing workspace created in the UI using the API or CLI driven workflow.
  * Alternatively, the tags that select the workspaces to use. See [Selecting Workspaces with Tags](cli-driven-workflow.md#selecting-workspaces-with-tags).

{% hint style="warning" %}
The `cloud` block is available in Terraform v1.1 and later. Previous versions can use the remote backend to configure the CLI workflow and migrate state.
{% endhint %}

### Selecting Workspaces with Tags <a href="#selecting-workspaces-with-tags" id="selecting-workspaces-with-tags"></a>

Instead of a single workspace `name`, the `cloud` block can select the workspaces to use by their [tags](../organizations/tags.md). The `tags` attribute accepts either of two forms:

* A list of tag keys, for example `tags = ["development", "networking"]`. A workspace matches when it has every key, whatever its values.
* A map of key/value tags, for example `tags = { env = "dev", app = "networking" }`. A workspace matches when it has every key with exactly that value.

Key/value tags let you tell apart workspaces that share the same keys. For example, with `myapp-dev` (`app = myapp`, `env = dev`) and `myapp-prod` (`app = myapp`, `env = prod`) in the organization, the following configuration always selects `myapp-dev`:

```
terraform {
  cloud {
    organization = "simple"
    hostname = "terrakube-api.example.com"

    workspaces {
      tags = {
        env = "dev"
        app = "myapp"
      }
    }
  }
}
```

When you run `terraform init`:

* If no workspace matches the tags, Terraform offers to create one, and the new workspace gets the tags from the configuration.
* If the selected workspace is missing some of the tags, Terraform adds them to the workspace. Tags that are not in the configuration, such as tags added in the UI, are kept.
* Selecting a workspace that does not match the tags, for example with `TF_WORKSPACE`, fails with `Invalid workspace selection`.

{% hint style="info" %}
When tags are set from the Terraform CLI, a workspace can have at most 10 tags.
{% endhint %}

### Terraform login

Authenticate the CLI against Terrakube with the standard login command (use `tofu login` for OpenTofu):

```
terraform login terrakube-api.example.com
```

Terraform opens your browser, you sign in with your identity provider, and the token is stored in the Terraform credentials file. Run it once for each hostname you use; if the registry has a different hostname from the API, log in to both (or put the same token in the credentials file for each).

#### Login through the Terrakube OAuth broker

{% hint style="info" %}
Available from Terrakube 2.34.0. It is disabled by default.
{% endhint %}

When enabled, Terrakube brokers the sign-in through Dex and shows a consent page where you choose how long the token lives (1 to 365 days). The CLI receives a normal, revocable personal access token, which you can see and delete in your [User Settings](../organizations/api-tokens.md). After a successful login the browser may show a connection error to `localhost`; this is expected, because Terraform closes its temporary listener as soon as it has the token.

Administrators enable it on the API with:

| Variable | Default | Description |
| --- | --- | --- |
| `TerraformLoginEnabled` | `false` | Turns the broker on and publishes the login discovery document. |
| `TerraformLoginApiUrl` | | Required when enabled. The public **HTTPS** URL of the API (for example `https://terrakube-api.example.com`), used for the OAuth callback and registry discovery. The API refuses to start with a non-HTTPS URL, except on `localhost`. |
| `TerraformLoginDefaultDays` | `30` | Token lifetime preselected on the consent page. |
| `TerraformLoginMaxDays` | `365` | Longest lifetime a user can pick. |
| `TerraformLoginDexTokenUrl`, `TerraformLoginDexJwkSetUrl` | | Optional. Dex token and signing-key endpoints the API calls, for example an in-cluster Dex service URL, when the public Dex URL is not reachable from the API. |

Also add `<TerraformLoginApiUrl>/oauth/callback` to the `redirectURIs` of your Dex static client; see [User Authentication (Dex)](../../getting-started/deployment/user-authentication-dex.md#terraform-login).

The broker uses PKCE, only redirects to the local CLI (`localhost`, ports 10000-10010), and validates Dex tokens against its signing keys.

### Terraform Init

After the authentication is completed you can run the following command to initialized the workspace

```
terraform init
```

The output will look like this:

```
terraform init

Initializing the backend...

Initializing provider plugins...
- Reusing previous version of hashicorp/random from the dependency lock file
- Using previously-installed hashicorp/random v3.4.3

Terraform has been successfully initialized!

You may now begin working with Terraform. Try running "terraform plan" to see
any changes that are required for your infrastructure. All Terraform commands
should now work.

If you ever set or change modules or backend configuration for Terraform,
rerun this command to reinitialize your working directory. If you forget, other
commands will detect it and remind you to do so if necessary.
```

If the initialization is successfull you should be able to see something like this:

### Implicit Workspace Creation <a href="#implicit-workspace-creation" id="implicit-workspace-creation"></a>

Terrakube will create a new workspace with the name you specify in the cloud block if it doesn’t already exist in your organization. You will see a message about this when you run terraform init.

### Terraform Plan

Once the terraform initialization is completed we should be able to run a terraform plan remotely using the following:

```
terraform plan
```

```
Running plan in the remote backend. Output will stream here. Pressing Ctrl-C
will stop streaming the logs, but will not stop the plan running remotely.

Preparing the remote plan...

To view this run in a browser, visit:
https://8080-azbuilder-terrakube-po7evw1u15x.ws-us86.gitpod.io/app/simple/workspace1/runs/1

Waiting for the plan to start...

Terrakube Remote Plan Execution



Terraform used the selected providers to generate the following execution
plan. Resource actions are indicated with the following symbols:
  + create

Terraform will perform the following actions:

  # random_string.random will be created
  + resource "random_string" "random" {
      + id               = (known after apply)
      + length           = 16
      + lower            = true
      + min_lower        = 0
      + min_numeric      = 0
      + min_special      = 0
      + min_upper        = 0
      + number           = true
      + numeric          = true
      + override_special = "/@£$"
      + result           = (known after apply)
      + special          = true
      + upper            = true
    }

Plan: 1 to add, 0 to change, 0 to destroy.
```

{% hint style="info" %}
You can use .tfvars file inside the workspace to upload the variables to the workspace automatically
{% endhint %}

If you go to the Terrakube UI, you will be able to see the workspace plan execution.

<figure><img src="../../.gitbook/assets/image (286).png" alt=""><figcaption></figcaption></figure>

### Terraform Apply

You can also execute a terraform apply and it will run inside Terrakube remotely.

```
terraform apply
Running apply in the remote backend. Output will stream here. Pressing Ctrl-C
will cancel the remote apply if it's still pending. If the apply started it
will stop streaming the logs, but will not stop the apply running remotely.

Preparing the remote apply...

To view this run in a browser, visit:
https://8080-azbuilder-terrakube-po7evw1u15x.ws-us86.gitpod.io/app/simple/workspace1/runs/2

Waiting for the plan to start...

Terrakube Remote Plan Execution



Terraform used the selected providers to generate the following execution
plan. Resource actions are indicated with the following symbols:
  + create

Terraform will perform the following actions:

  # random_string.random will be created
  + resource "random_string" "random" {
      + id               = (known after apply)
      + length           = 16
      + lower            = true
      + min_lower        = 0
      + min_numeric      = 0
      + min_special      = 0
      + min_upper        = 0
      + number           = true
      + numeric          = true
      + override_special = "/@£$"
      + result           = (known after apply)
      + special          = true
      + upper            = true
    }

Plan: 1 to add, 0 to change, 0 to destroy.


Do you want to perform these actions in workspace "workspace1"?
  Terraform will perform the actions described above.
  Only 'yes' will be accepted to approve.

  Enter a value: yes

random_string.random: Creating...
random_string.random: Creation complete after 0s [id=QvsEmtO7WoeJJcAJ]

Apply complete! Resources: 1 added, 0 changed, 0 destroyed.
```

<figure><img src="../../.gitbook/assets/image (323).png" alt=""><figcaption></figcaption></figure>

{% hint style="warning" %}
The terraform apply can only be approved using the terraform CLI. You wont be able to approve the job inside the UI.
{% endhint %}

Terraform Destroy

Executing the terraform destroy is also possible.

```
terraform destroy
Running apply in the remote backend. Output will stream here. Pressing Ctrl-C
will cancel the remote apply if it's still pending. If the apply started it
will stop streaming the logs, but will not stop the apply running remotely.

Preparing the remote apply...

To view this run in a browser, visit:
https://8080-azbuilder-terrakube-po7evw1u15x.ws-us86.gitpod.io/app/simple/workspace1/runs/3

Waiting for the plan to start...

Terrakube Remote Plan Execution


random_string.random: Refreshing state... [id=QvsEmtO7WoeJJcAJ]

Terraform used the selected providers to generate the following execution
plan. Resource actions are indicated with the following symbols:
  - destroy

Terraform will perform the following actions:

  # random_string.random will be destroyed
  - resource "random_string" "random" {
      - id               = "QvsEmtO7WoeJJcAJ" -> null
      - length           = 16 -> null
      - lower            = true -> null
      - min_lower        = 0 -> null
      - min_numeric      = 0 -> null
      - min_special      = 0 -> null
      - min_upper        = 0 -> null
      - number           = true -> null
      - numeric          = true -> null
      - override_special = "/@£$" -> null
      - result           = "QvsEmtO7WoeJJcAJ" -> null
      - special          = true -> null
      - upper            = true -> null
    }

Plan: 0 to add, 0 to change, 1 to destroy.


Do you really want to destroy all resources in workspace "workspace1"?
  Terraform will destroy all your managed infrastructure, as shown above.
  There is no undo. Only 'yes' will be accepted to confirm.

  Enter a value: yes

random_string.random: Destroying... [id=QvsEmtO7WoeJJcAJ]
random_string.random: Destruction complete after 0s

Apply complete! Resources: 0 added, 0 changed, 1 destroyed.
```

Inside the UI the terraform destroy operation will look like this.

<figure><img src="../../.gitbook/assets/image (210).png" alt=""><figcaption></figcaption></figure>

{% hint style="warning" %}
The terraform destroy can only be approved using the terraform CLI. You wont be able to approve the job inside the UI.
{% endhint %}
