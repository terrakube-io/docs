# 🌟 Getting started

Firts we need to clone the docker compose reposiory to run the API.

```
git clone https://github.com/AzBuilder/terrakube-docker-compose
cd local
docker-compose up
```

After a coubple of seconds the API will be up and running and can take request, now we can open the postman collection inside the folder "getting-started"

{% hint style="warning" %}
In docker compose terrakube security is disable by default and it is not connected to any Azure Active Directory, it is also using an in-memory, use this just for testing
{% endhint %}

### Organizations

Terrakube is based on Organization, we can have one or multiple organizations inside Terrakube and we use an organization to save workspaces, jobs, templates, modules, providers and teams.

To create it we will use the request inside the postman collection "Step 1 - Organization"

![](<../.gitbook/assets/image (18).png>)

Request to create an Organization called "Terrakube":

![](<../.gitbook/assets/image (19).png>)

We should receive a response like the following:

![](<../.gitbook/assets/image (39).png>)

Now we have the basic element in Terrakube.

### Teams.

Inside an Organization we can have multiple teams working, each team can have access to different features inside an organization.

To create it we will use the request inside the postman collection "Step 2 - Teams"

![](<../.gitbook/assets/image (38).png>)

Request to create a team called "Terrakube\_Team" with access to all the features:

![](<../.gitbook/assets/image (40).png>)

{% hint style="warning" %}
The team name should be an Azure Active Directory Group, all team members within the group will have access.
{% endhint %}

{% hint style="warning" %}
In real life scenarios only member of the Terrakube Admin group have access to create and manage teams
{% endhint %}

Respose:

![](<../.gitbook/assets/image (30).png>)

### Templates

This is the basic flow a job inside Terrakube will execute when running a workspace jobs. Templates allows you to define different logic using "Terrakube Configuration Language" using a yaml and small scripts written in Bash or Groovy that can be reused accross an organization. To understand better templates lets create some examples:

{% hint style="info" %}
Templates are sent to the API in base64 encoding.
{% endhint %}

To create it we will use the request inside the postman collection "Step 3 - Templates"

![](<../.gitbook/assets/image (8).png>)

#### Template 1 - Basic Terraform Plan-Apply

This is an small template that can be used to run a terraform plan and later run a terraform apply:

Teamplate body:

```
flow:
  - type: "terraformPlan"
    step: 100
  - type: "terraformApply"
    step: 200
```

Request:

![](<../.gitbook/assets/image (25).png>)

Response:

![](<../.gitbook/assets/image (22).png>)

Now we have a template that we can reuse when running jobs accross an organization, lets define a couple of more templates.

#### Template 2 - Terraform Basic Destroy.

This is a small template that can be used to destoy the terraform infraestructure.

Template body:

```
flow:
  - type: "terraformDestroy"
    step: 100
```

Request:

![](<../.gitbook/assets/image (17).png>)

Response:

![](<../.gitbook/assets/image (35).png>)

Now our firts organization have to templates, one to run a terraform plan/apply and one for a terraform destroy. Lets create a couple of more templates.

#### Template 3 - Calculate cost with Infracost Template

Terraform has a lot of open source tools that we can use inside Terrakube using **template extensions,** Terrakube has an open source repository where extension can be created and share between our community. Lets use some of the available extension to create a template that can calculate the cost of our infrastructure using "**Infracost**".

Template body:

```
flow:
- type: "terraformPlan"
  step: 100
  commands:
    - runtime: "GROOVY"
      priority: 100
      after: true
      script: |
        import Infracost

        String credentials = "version: \"0.1\"\n" +
                "api_key: $INFRACOST_KEY \n" +
                "pricing_api_endpoint: https://pricing.api.infracost.io"

        new Infracost().loadTool(
           "$workingDirectory",
           "$bashToolsDirectory", 
           "0.9.11",
           credentials)
        "Infracost Download Completed..."
    - runtime: "BASH"
      priority: 200
      after: true
      script: |
        terraform show -json terraformLibrary.tfPlan > plan.json 
        infracost breakdown --path plan.json
```

In this template we are defining a flow where a terraform plan will be executed for the workspace, once the terraform plan is finished Terrakube will import the Infracost extension from the open source repository and calculate the cost.

Request:

![](<../.gitbook/assets/image (9) (1).png>)

Response:

![](<../.gitbook/assets/image (23).png>)

#### Template 4 - Static Code Analysis Terrascan

Lets define a new template that use **Terrascan** open source tool to run a static code analisys of our terraform code.

Template body:

```
flow:
  - type: "customScripts"
    step: 100
    commands:
      - runtime: "GROOVY"
        priority: 100
        after: true
        script: |
          import Terrascan

          new Terrascan().loadTool(
            "$workingDirectory",
            "$bashToolsDirectory",
            "1.12.0")
          "Terrascan Download Completed..."
      - runtime: "BASH"
        priority: 200
        after: true
        script: |
          cd $workingDirectory;
          terrascan scan -i terraform -t azure;
```

This template is importing the **Terrascan** extension and running a analisys for one particular workspace.

**Template 5 - Static Code Analysis Snyk**

Lets create a template that is using Snyk open source to run an analysis of our terraform code.

Template body:

```
flow:
  - type: "customScripts"
    step: 100
    commands:
      - runtime: "GROOVY"
        priority: 100
        after: true
        script: |
          import Snyk

          new Snyk().loadTool(
            "$workingDirectory",
            "$bashToolsDirectory",
            "1.831.0")
          "Snyk Download Completed..."
      - runtime: "BASH"
        priority: 200
        after: true
        script: |
          cd $workingDirectory;
          snyk iac test .;
```

Request:

![](<../.gitbook/assets/image (27).png>)

Response:

![](<../.gitbook/assets/image (21).png>)

Now we have the following template available inside our organization:

* Terraform Plan/Apply
* Terraform Destroy
* Terraform Cost with Infracost
* Terraform Static Code Analysis with Terrascan
* Terraform Static Code Analysis with Snyk

Lets create some workspace where we can use our templates.

{% hint style="info" %}
For more information about Terrakube extension please refer to the followin Github [repository](https://github.com/AzBuilder/terrakube-extensions). If you want to create a new extension feel free to create an issue and send a pull request. You can even for the repository and create some private extension.
{% endhint %}

### Worspaces

A workspace is a git repository where we store our terraform code, we can define all the terraform resources or we can just call a terraform module that is available inside Terrakube modules.

{% hint style="info" %}
Workspace Git repository can be public or private using the following providers: Github, Gitlab, Azure DevOps or Bitbucket. If you are using a private git respository please refer to the following[ document](broken-reference/).
{% endhint %}

To create it we will use the request inside the postman collection "Step 4 - Worspace"

![](<../.gitbook/assets/image (37).png>)

Lets create a simple workspace using the following terraform code.

```
# This resource will destroy (potentially immediately) after null_resource.next
resource "null_resource" "previous" {}

resource "time_sleep" "wait_30_seconds" {
  depends_on = [null_resource.previous]

  create_duration = "30s"
}

# This resource will create (at least) 30 seconds after null_resource.previous
resource "null_resource" "next" {
  depends_on = [time_sleep.wait_30_seconds]
}
```

Request.

![](<../.gitbook/assets/image (36).png>)

Response.

![](<../.gitbook/assets/image (29).png>)

### Worspaces Schedule

When a workspace is created inside Terrakube we can define some schedules, this can be usefull for example when running cloud infrastructure and we need to create some development environment every day at 8 AM and destroy the environment every day at 5 PM when our IT team stop working.

Request

![](<../.gitbook/assets/image (13).png>)

{% hint style="info" %}
This schedule will run the workspace using one template every 30 minutes
{% endhint %}

The Terrakube scheduler is based in "**Quartz**". You can create different schedules for your workspace. Please refer to the [following information](http://www.quartz-scheduler.org) to define the "**cron expression**" or you can use the following link to build the [expresion](https://www.freeformatter.com/cron-expression-generator-quartz.html).

### Worspace History

When Terrakube runs a job it will start saving all the states and changes inside the workspace history. To validate all the state changes we can use the workspace history endpoint.

Request

![](<../.gitbook/assets/image (28).png>)

### Worspace (Azure/GCP/AWS)

To understan how you can create workspace for any cloud provider please refer to the following examples inside the postman collection:

![](<../.gitbook/assets/image (24).png>)

Basically to use any terraform provider we just need to define the worspace environment variables.

This is an example of an Azure workspace.

![](<../.gitbook/assets/image (12).png>)

For more information about the workspace please check the api methods for:

* [Workspace](methods/workspace.md)
* [Variables](methods/variables.md)
* [History](methods/history.md)
* [Schedules](methods/schedule.md)

### Jobs

After we have a workspace we can star creating jobs for our workspaces using the templates the we have define earlier.

To create it we will use the request inside the postman collection "Step 5 - Jobs"

![](<../.gitbook/assets/image (34).png>)

Request:

![](<../.gitbook/assets/image (32).png>)

Response:

![](<../.gitbook/assets/image (15).png>)

After a couple of minutes Terrakube should have completed the job execution and we can use the following endpoint to check the output

![](<../.gitbook/assets/image (33).png>)

The output should be similar to the following:

```
{
    "data": {
        "type": "job",
        "id": "1",
        "attributes": {
            "approvalTeam": null,
            "createdBy": "local@demo.com",
            "createdDate": "2022-04-16T16:21Z",
            "output": " Step 15cc4aaf-98cc-48f3-a7eb-eaf7eabb53a1 completed\n",
            "status": "completed",
            "tcl": "ZmxvdzoKICAtIHR5cGU6ICJ0ZXJyYWZvcm1QbGFuIgogICAgc3RlcDogMTAwCiAgLSB0eXBlOiAidGVycmFmb3JtQXBwbHkiCiAgICBzdGVwOiAyMDA=",
            "templateReference": "f8347f0e-145e-4bc4-800a-de1fe134fb74",
            "terraformPlan": "http://localhost:8090/state/de64af52-f676-4e62-bfa3-775d5a8b8dc0/464105d6-d6b9-47b5-9abb-fc541cba3233/1/c7f1dbfd-19ff-4a75-bb77-3b0fb82b4db8/terraformLibrary.tfPlan",
            "updatedBy": "local@demo.com",
            "updatedDate": "2022-04-16T16:24Z"
        },
        "relationships": {
            "organization": {
                "data": {
                    "type": "organization",
                    "id": "de64af52-f676-4e62-bfa3-775d5a8b8dc0"
                }
            },
            "step": {
                "data": [
                    {
                        "type": "step",
                        "id": "15cc4aaf-98cc-48f3-a7eb-eaf7eabb53a1"
                    },
                    {
                        "type": "step",
                        "id": "c7f1dbfd-19ff-4a75-bb77-3b0fb82b4db8"
                    }
                ]
            },
            "workspace": {
                "data": {
                    "type": "workspace",
                    "id": "464105d6-d6b9-47b5-9abb-fc541cba3233"
                }
            }
        }
    },
    "included": [
        {
            "type": "step",
            "id": "c7f1dbfd-19ff-4a75-bb77-3b0fb82b4db8",
            "attributes": {
                "name": "Running Step100",
                "output": "http://localhost:8090/output/de64af52-f676-4e62-bfa3-775d5a8b8dc0/1/c7f1dbfd-19ff-4a75-bb77-3b0fb82b4db8.tfoutput",
                "status": "completed",
                "stepNumber": 100
            },
            "relationships": {
                "job": {
                    "data": {
                        "type": "job",
                        "id": "1"
                    }
                }
            }
        },
        {
            "type": "step",
            "id": "15cc4aaf-98cc-48f3-a7eb-eaf7eabb53a1",
            "attributes": {
                "name": "Running Step200",
                "output": "http://localhost:8090/output/de64af52-f676-4e62-bfa3-775d5a8b8dc0/1/15cc4aaf-98cc-48f3-a7eb-eaf7eabb53a1.tfoutput",
                "status": "completed",
                "stepNumber": 200
            },
            "relationships": {
                "job": {
                    "data": {
                        "type": "job",
                        "id": "1"
                    }
                }
            }
        }
    ]
}
```

### Terrakube Open API Specification.

If you wan to check all the available methods you can use the following enpoint.

![](<../.gitbook/assets/image (31).png>)

![](<../.gitbook/assets/image (26).png>)
