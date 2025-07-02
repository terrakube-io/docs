# Ephemeral Workspaces

The following will show how easy is to implement an ephemeral workspace using Terrakube custom schedules and templates with the remote CLI-driven workflow.

The first step will be to create a new organization, lets call it "playground".

<figure><img src="../../.gitbook/assets/image (79).png" alt=""><figcaption></figcaption></figure>

Once we have the playground organization, we need to add a team with access to create templates like the following:

<figure><img src="../../.gitbook/assets/image (80).png" alt=""><figcaption></figcaption></figure>

We will also need a team with access to create/delete a workspace only, like the following:

<figure><img src="../../.gitbook/assets/image (81).png" alt=""><figcaption></figcaption></figure>

{% hint style="warning" %}
Teams names will depend on your Dex configuration.
{% endhint %}

Once we have the two teams in our new playground organization, we can proceed and create a new template that we will be using to auto destroy all the workspace:

<figure><img src="../../.gitbook/assets/image (89).png" alt=""><figcaption></figcaption></figure>

Lets call it "delete-playground" and it will have the following code:

```yaml
flow:
  - type: "terraformDestroy"
    name: "Destroy Playground"
    step: 100
  - type: "disableWorkspace"
    name: "Delete Workspace"
    step: 200

```

<figure><img src="../../.gitbook/assets/image (90).png" alt=""><figcaption></figcaption></figure>

Now we can update the default template that is used when we are using the remote CLI-driven workflow, this template is inside every organization by default and is called "**Terraform-Plan/Apply-Cli**", usually we don't need to update this template but we will do some small changes to enable ephemeral workspaces in the playground organization.

We need to go the the templates inside the organization settings and edit the template

<figure><img src="../../.gitbook/assets/image (82).png" alt=""><figcaption></figcaption></figure>

We will add a new step in the template, this will allow to schedule a job that will be using the "delete-playground" template that we have created above.

<figure><img src="../../.gitbook/assets/image (84).png" alt=""><figcaption></figcaption></figure>

We need to use the following template code:

```yaml
flow:
- type: "terraformPlan"
  name: "Terraform Plan from Terraform CLI"
  step: 100
- type: "approval"
  name: "Approve Plan from Terraform CLI"
  step: 150
  team: "TERRAFORM_CLI"
- type: "terraformApply"
  name: "Terraform Apply from Terraform CLI"
  step: 200
- type: "scheduleTemplates"
  step: 300
  name: "Setup auto destroy"
  templates:
    - name: "delete-playground"
      schedule: "0 0/5 * ? * * *"

```

If we pay special attention we just add a new section where we define that the schedule will run every five minutes after the Terraform apply is completed.

In this example we will schedule the template every five minutes for testing purposes.

```yaml
  name: "Setup auto destroy"
  templates:
    - name: "delete-playground"
      schedule: "0 0/5 * ? * * *"
```

{% hint style="info" %}
The schedule is using Quartz format, to learn more about this use this [link](https://www.freeformatter.com/cron-expression-generator-quartz.html).&#x20;
{% endhint %}

Now we need to define our Terraform code, lets use the following simple example:

```
terraform {
  cloud {
    organization = "playground"
    hostname = "8080-terrakube-io-terrakube-h128dcdc7l1.ws-us105.gitpod.io"

    workspaces {
      tags = ["myplayground", "example"]
    }
  }
}

resource "null_resource" "previous" {}

resource "time_sleep" "wait_5_seconds" {
  depends_on = [null_resource.previous]

  create_duration = "5s"
}

resource "null_resource" "next" {
  depends_on = [time_sleep.wait_5_seconds]
}

output "creation_time" {
    value = time_sleep.wait_5_seconds.create_duration
}
```

Run **terraform login** to connect to our Terrakube instance:

```
terraform login 8080-terrakube-io-terrakube-h128dcdc7l1.ws-us105.gitpod.io
```

Now we can run **terraform init** to initialize our workspace inside the playground organization, lets use "myplayground" for the name of our new workspace

<figure><img src="../../.gitbook/assets/image (85).png" alt=""><figcaption></figcaption></figure>

Let's run terraform apply and create our resources:

```bash
Preparing the remote apply...

To view this run in a browser, visit:
https://8080-terrakube-io-terrakube-h128dcdc7l1.ws-us105.gitpod.io/app/playground/myplayground/runs/1

Waiting for the plan to start...

***************************************
Running Terraform PLAN
***************************************

Terraform used the selected providers to generate the following execution
plan. Resource actions are indicated with the following symbols:
  + create

Terraform will perform the following actions:

  # null_resource.next will be created
  + resource "null_resource" "next" {
      + id = (known after apply)
    }

  # null_resource.previous will be created
  + resource "null_resource" "previous" {
      + id = (known after apply)
    }

  # time_sleep.wait_5_seconds will be created
  + resource "time_sleep" "wait_5_seconds" {
      + create_duration = "5s"
      + id              = (known after apply)
    }

Plan: 3 to add, 0 to change, 0 to destroy.

Changes to Outputs:
  + creation_time = "5s"

Do you want to perform these actions in workspace "myplayground"?
  Terraform will perform the actions described above.
  Only 'yes' will be accepted to approve.

  Enter a value: yes

null_resource.previous: Creating...
null_resource.previous: Creation complete after 0s [id=7198759863280029870]
time_sleep.wait_5_seconds: Creating...
time_sleep.wait_5_seconds: Creation complete after 5s [id=2023-10-18T16:05:14Z]
null_resource.next: Creating...
null_resource.next: Creation complete after 0s [id=855270182201609076]

Apply complete! Resources: 3 added, 0 changed, 0 destroyed.

Outputs:

creation_time = "5s"

```

Our new workspace is created and if we go the organization we can see all the information

<figure><img src="../../.gitbook/assets/image (86).png" alt=""><figcaption></figcaption></figure>

The job will be running to create the resources:

<figure><img src="../../.gitbook/assets/image (88).png" alt=""><figcaption></figcaption></figure>

We can see that our job is completed and the setup auto destroy have created a new schedule for our workspace:

<figure><img src="../../.gitbook/assets/image (93).png" alt=""><figcaption></figcaption></figure>

We could go to the schedules tab:

<figure><img src="../../.gitbook/assets/image (87).png" alt=""><figcaption></figcaption></figure>

This schedule will run in 5 minutes:

<figure><img src="../../.gitbook/assets/image (92).png" alt=""><figcaption></figcaption></figure>

After waiting for 5 minutes we will see that Terrakube have created a new Job automatically

<figure><img src="../../.gitbook/assets/image (94).png" alt=""><figcaption></figcaption></figure>

If we check the details for the new job we can see that a terraform destroy will be executed:

<figure><img src="../../.gitbook/assets/image (95).png" alt=""><figcaption></figcaption></figure>

All of the workspace resources are deleted and the workspace will be deleted automatically after the destroy is completed.

<figure><img src="../../.gitbook/assets/image (96).png" alt=""><figcaption></figcaption></figure>

Once the Job is completed, our workspace information is deleted from Terrakube

<figure><img src="../../.gitbook/assets/image (97).png" alt=""><figcaption></figcaption></figure>

If we are using AWS, AZURE, GCP or any other terraform provider that allow to inject the credentials using environment variables we could use "global variables" to define those.&#x20;

Global variables will be injected automatically to any workspace inside the playground organization.

<figure><img src="../../.gitbook/assets/image (98).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (99).png" alt=""><figcaption></figcaption></figure>
