# CLI-driven Workflow

Cli-driven workflow allow to use Terrakube remotely using only the Terraform CLI.

{% hint style="info" %}
Terrakube creates default templates for CLI-Driven workflow when you create a new organization, if you delete those templates you won't be able to run this workflow, check [default templates](../organizations/templates/default-templates.md#cli-driven-templates) for more information.
{% endhint %}

{% hint style="warning" %}
&#x20;At the moment all the terraform operations are executed remotely in Terrakube
{% endhint %}

### Configuration

To use the CLI-driven workflow the firts step will be to setup our project to use the terraform remote state in Terrakube as the following example:

```
terraform {
  backend "remote" {
    hostname = "8080-azbuilder-terrakube-po7evw1u15x.ws-us86.gitpod.io"
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

* Hostname
  * This should be the Terrakube API
* Organization
  * This should be the Terrakube organization where the workspace will be created.&#x20;
* Workspace
  * The name of the workspace to be created. Or you can use an existing workspace created in the UI using the API or CLI driven workflow.

### Terraform login.

Once we have added the terraform remote state configuration we will need to authenticate to Terrakube using the following command with the Terrakube API as the following:

```
terraform login 8080-azbuilder-terrakube-3xqsq270uq1.ws-us82.gitpod.io
```

The output will look something like the following:

<figure><img src="../../.gitbook/assets/image (3) (1) (1).png" alt=""><figcaption></figcaption></figure>

```
terraform login 8080-azbuilder-terrakube-po7evw1u15x.ws-us86.gitpod.io
Terraform will request an API token for 8080-azbuilder-terrakube-po7evw1u15x.ws-us86.gitpod.io using OAuth.

This will work only if you are able to use a web browser on this computer to
complete a login process. If not, you must obtain an API token by another
means and configure it in the CLI configuration manually.

If login is successful, Terraform will store the token in plain text in
the following file for use by subsequent commands:
    C:\Users\XXXXXX\AppData\Roaming\terraform.d\credentials.tfrc.json

Do you want to proceed?
  Only 'yes' will be accepted to confirm.

  Enter a value: yes

Terraform must now open a web browser to the login page for 8080-azbuilder-terrakube-po7evw1u15x.ws-us86.gitpod.io.

If a browser does not open this automatically, open the following URL to proceed:
    https://5556-azbuilder-terrakube-po7evw1u15x.ws-us86.gitpod.io/dex/auth?scope=openid+profile+email+offline_access+groups&client_id=example-app&code_challenge=YYe059U9HyeMy1-RmuEzSajPJYGdcY4IkNr0pZz4LZ8&code_challenge_method=S256&redirect_uri=http%3A%2F%2Flocalhost%3A10000%2Flogin&response_type=code&state=f8b6da79-35f3-2b92-d8d8-179cda386f91

Terraform will now wait for the host to signal that login was successful.


---------------------------------------------------------------------------------

Success! Terraform has obtained and saved an API token.

The new API token will be used for any future Terraform command that must make
authenticated requests to 8080-azbuilder-terrakube-po7evw1u15x.ws-us86.gitpod.io.
```

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

<figure><img src="../../.gitbook/assets/image (1) (1) (2).png" alt=""><figcaption></figcaption></figure>

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

<figure><img src="../../.gitbook/assets/image (6) (1) (1).png" alt=""><figcaption></figcaption></figure>

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

<figure><img src="../../.gitbook/assets/image (2) (2).png" alt=""><figcaption></figcaption></figure>

{% hint style="warning" %}
The terraform destroy can only be approved using the terraform CLI. You wont be able to approve the job inside the UI.
{% endhint %}
