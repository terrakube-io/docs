# Getting started

You can have a complete development environment to test the API with one click with Gitpod using the following button.

[![Gitpod ready-to-code](https://camo.githubusercontent.com/791bd446c60d39e9a296e8d02837c81fe0af6108f02a04afbc93edda0cb93ad6/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f476974706f642d72656164792d2d746f2d2d636f64652d626c75653f6c6f676f3d676974706f64267374796c653d666c61742d737175617265)](https://gitpod.io/#https://github.com/AzBuilder/terrakube)

{% hint style="warning" %}
Gitpod is [free for 50 hours](https://www.gitpod.io/pricing) every month, so you dont have to worry for testing the application.
{% endhint %}

After a couple of seconds the Gitpod environment will be ready and you will need to start the API, to do this just select "Run and Debug" in the left menu in VS Code and start the application using the "Terrakube" option

<figure><img src="../.gitbook/assets/image (310).png" alt=""><figcaption></figcaption></figure>

After some seconds Terrakube API will be up and running.

<figure><img src="../.gitbook/assets/image (267).png" alt=""><figcaption></figcaption></figure>

Now you can select the "Thunder Client" and open the "Getting Started" collection to test the API

<figure><img src="../.gitbook/assets/image (384).png" alt=""><figcaption></figcaption></figure>

### Authentication

The first step will be to get the authorization token for the Terrakube API.

Execute the request "Get Device Code" and open the URL in field "verification\_uri\_complete" inside the response to get the authentication token

<figure><img src="../.gitbook/assets/image (277).png" alt=""><figcaption></figcaption></figure>

When you open the "verification\_\_uri\_\_complete" field you should see a similar page.

<figure><img src="../.gitbook/assets/image (249).png" alt=""><figcaption></figcaption></figure>

Authenticate using the user "admin@example.com" with password "admin"

<figure><img src="../.gitbook/assets/image (378).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (335).png" alt=""><figcaption></figcaption></figure>

Finally get the access token for the API using the request "Get Access Token"

<figure><img src="../.gitbook/assets/image (161).png" alt=""><figcaption></figcaption></figure>

Now we can start using the API

### Organizations

Terrakube is based on Organization, we can have one or multiple organizations inside Terrakube and we use an organization to save workspaces, jobs, templates, modules, providers and teams.

To create it we will use the request inside the collection "Step 1 - Organization"

<figure><img src="../.gitbook/assets/image (119).png" alt=""><figcaption></figcaption></figure>

Request to create an Organization called "Hashicorp":

We should receive a response like the following:

<figure><img src="../.gitbook/assets/image (402).png" alt=""><figcaption></figcaption></figure>

Now we have the basic element in Terrakube to start testing all the endpoints.

### Teams.

Inside an Organization we can have multiple teams working, each team can have access to different features inside an organization.

To create it we will use the request inside the collection "Step 2 - Teams"

<figure><img src="../.gitbook/assets/image (408).png" alt=""><figcaption></figcaption></figure>

This will create a team called "TERRAKUBE\_DEVELOPERS" with access to all the features and you should get a response like the following:

<figure><img src="../.gitbook/assets/image (263).png" alt=""><figcaption></figcaption></figure>

{% hint style="warning" %}
The team name should be a valid "group" name in Azure Active Directory, Google Cloud Identity, Github, Gitlab, OIDC, Amazon Cognito, etc. This will be based on Dex Configuration. For this example we are using Terrakube with a preconfigured LDAP.
{% endhint %}

### Templates

This is the basic flow a job inside Terrakube will execute when running a workspace jobs. Templates allows you to define different logic using "Terrakube Configuration Language" using a yaml and small scripts written in Bash or Groovy that can be reused across an organization. To understand better templates lets create some examples:

{% hint style="info" %}
Templates are sent to the API in base64 encoding.
{% endhint %}

To create it we will use the request inside the postman collection "Step 3 - Templates"

<figure><img src="../.gitbook/assets/image (143).png" alt=""><figcaption></figcaption></figure>

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

<figure><img src="../.gitbook/assets/image (348).png" alt=""><figcaption></figcaption></figure>

Response:

<figure><img src="../.gitbook/assets/image (285).png" alt=""><figcaption></figcaption></figure>

Now we have a template that we can reuse when running jobs across an organization, lets define a couple of more templates.

#### Template 2 - Terraform Basic Destroy.

This is a small template that can be used to destroy the terraform infraestructure.

Template body:

```
flow:
  - type: "terraformDestroy"
    step: 100
```

Request:

<figure><img src="../.gitbook/assets/image (272).png" alt=""><figcaption></figcaption></figure>

Response:

<figure><img src="../.gitbook/assets/image (365).png" alt=""><figcaption></figcaption></figure>

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

<figure><img src="../.gitbook/assets/image (282).png" alt=""><figcaption></figcaption></figure>

Response:

<figure><img src="../.gitbook/assets/image (379).png" alt=""><figcaption></figcaption></figure>

#### Template 4 - Static Code Analysis Terrascan

Lets define a new template that use **Terrascan** open source tool to run a static code analysis of our terraform code.

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

Request:

<figure><img src="../.gitbook/assets/image (224).png" alt=""><figcaption></figcaption></figure>

Response:

<figure><img src="../.gitbook/assets/image (220).png" alt=""><figcaption></figcaption></figure>

This template is importing the **Terrascan** extension and running a analysis for one particular workspace.

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

<figure><img src="../.gitbook/assets/image (154).png" alt=""><figcaption></figcaption></figure>

Response:

<figure><img src="../.gitbook/assets/image (244).png" alt=""><figcaption></figcaption></figure>

Now we have the following template available inside our organization:

* Terraform Plan/Apply
* Terraform Destroy
* Terraform Cost with Infracost
* Terraform Static Code Analysis with Terrascan
* Terraform Static Code Analysis with Snyk

Lets create some workspace where we can use our templates.

{% hint style="info" %}
For more information about Terrakube extension please refer to the following Github [repository](https://github.com/AzBuilder/terrakube-extensions). If you want to create a new extension feel free to create an issue and send a pull request. You can even for the repository and create some private extension.
{% endhint %}

### workspaces

A workspace is a folder inside a git repository where we store our terraform code, we can define all the terraform resources or we can just call a terraform module that is available inside Terrakube modules.

{% hint style="info" %}
Workspace Git repository can be public or private using the following providers: Github, Gitlab, Azure DevOps, Bitbucket or SSH keys (rsa and ed25519). If you are using a private git respository please refer to the following[ document](https://github.com/AzBuilder/docs/blob/master/api/broken-reference/README.md).
{% endhint %}

To create it we will use the request inside the postman collection "Step 4 - workspace"

<figure><img src="../.gitbook/assets/image (265).png" alt=""><figcaption></figcaption></figure>

Lets create a very simple workspace using the following terraform code inside this public git repository "[https://github.com/AzBuilder/terrakube-docker-compose.git](https://github.com/AzBuilder/terrakube-docker-compose.git)" using the root folder "/"

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

{% hint style="info" %}
You can have multiple workspaces using a single git repository using folders and changing the field "folder" in the request.
{% endhint %}

Request.

<figure><img src="../.gitbook/assets/image (356).png" alt=""><figcaption></figcaption></figure>

Response.

<figure><img src="../.gitbook/assets/image (278).png" alt=""><figcaption></figcaption></figure>

### workspaces Schedule

When a workspace is created inside Terrakube we can define some schedules, this can be useful for example when running cloud infrastructure and we need to create some development environment every day at 8 AM and destroy the environment every day at 5 PM when our IT team stop working.

Request

<figure><img src="../.gitbook/assets/image (145).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
This schedule will run the workspace using one template every minute
{% endhint %}

The Terrakube scheduler is based in "**Quartz**". You can create different schedules for your workspace. Please refer to the [following information](http://www.quartz-scheduler.org) to define the "**cron expression**" or you can use the following link to build the [expression](https://www.freeformatter.com/cron-expression-generator-quartz.html).

### workspace History

When Terrakube runs a job it will start saving all the states and changes inside the workspace history. To validate all the state changes we can use the workspace history endpoint.

Request

<figure><img src="../.gitbook/assets/image (104).png" alt=""><figcaption></figcaption></figure>

### workspace (Azure/GCP/AWS)

To understand how you can create workspace for any cloud provider please refer to the following examples inside the collection:

<figure><img src="../.gitbook/assets/image (349).png" alt=""><figcaption></figcaption></figure>

Basically to use any terraform provider we just need to define the workspace environment variables.

This is an example of an Azure workspace.

<figure><img src="../.gitbook/assets/image (238).png" alt=""><figcaption></figcaption></figure>

For more information about the workspace please check the api methods for:

* [Workspace](methods/workspace.md)
* [Variables](methods/variables.md)
* [History](methods/history.md)
* [Schedules](methods/schedule.md)

### Jobs

After we have a workspace we can start creating jobs for our workspaces using the templates that we have define earlier.

To create it we will use the request inside the postman collection "Step 5 - Jobs"

<figure><img src="../.gitbook/assets/image (155).png" alt=""><figcaption></figcaption></figure>

Request:

<figure><img src="../.gitbook/assets/image (333).png" alt=""><figcaption></figcaption></figure>

After a couple of minutes Terrakube should have completed the job execution

The job output should be similar to the following:

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

<figure><img src="../.gitbook/assets/image (372).png" alt=""><figcaption></figcaption></figure>
