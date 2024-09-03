# Updates

### July 2024 (2.22.0)

Welcome to the 2.22.0 release of Terrakube! As the summer season comes to a close, we’re focusing on enhancing the stability of the recent features we’ve introduced. While this is a smaller update compared to previous releases, we’re excited to see an increase in contributions from the community. Let’s dive into the key highlights:

#### Ephemeral Agents

Before this version, the executor component—responsible for running jobs like plan, apply, destroy, etc.—was more static, with the API always running and listening for new jobs. In this update, we’re introducing a more dynamic and efficient approach: ephemeral agents. Now, a new executor is created for each job and is disposed of once the job is finished. This enhancement leverages Kubernetes jobs to schedule each run, plan, and apply, making the process more resource-efficient and scalable.

![image](https://github.com/user-attachments/assets/36c2970f-816f-45e1-a78d-cb21fbbec17a)

![image](https://github.com/user-attachments/assets/166f3481-8bd4-496f-8e1d-066787c4de5d)

If you’re interested in switching to ephemeral agents, follow our [guide](https://docs.terrakube.io/getting-started/deployment/ephemeral-agents) to get started.

#### Enhanced GitHub Integration with Status Checks

In this version, we’ve enhanced the GitHub integration by adding status checks. Previously, every push to the branch configured in your Terrakube workspace would trigger a Terraform/OpenTofu apply, but there was no way to monitor the job’s progress or navigate to the workspaces directly from GitHub. With this update, you can now see the status (completed or failed) of the job directly within GitHub, and by clicking the "Details" button, you can easily navigate to Terrakube to view more information.

![image](https://github.com/user-attachments/assets/8d95d6b3-3e4b-4e3d-89ae-d7276d8cf967)

Thanks to @stanleyz for this contribution!

#### ARM64 Support for Terrakube Images

In this version, we’ve converted all Terrakube images for the UI, API, and executor to multi-architecture images. This means you can now run Terrakube on both Linux/AMD64 and Linux/ARM64 platforms, providing greater flexibility and compatibility across different environments.

For more details about our official images, take a look at our [documentation](https://docs.terrakube.io/getting-started/docker-images).

#### Custom Releases Endpoint for OpenTofu

As OpenTofu continues to gain popularity, we’ve added a new feature in this version that allows you to specify a custom releases endpoint. By default, Terrakube uses the official OpenTofu repository to list available versions. With this update, you now have the option to define a custom releases endpoint, giving you more control over the versions you allow internally for your workspaces. This also enables you to create and use custom builds from OpenTofu in your Terrakube jobs.

Thanks to @gespinozat for this contribution!

#### Multiple Branches Support in a Workspace

In this version, we’ve added the option to specify multiple branches within a single workspace. This feature is particularly useful if you need to run Terrakube jobs across different branches within the same workspace, such as during development. It provides greater flexibility and control over your infrastructure management process, allowing you to test and deploy changes in various branches without needing separate workspaces.

Thanks @akozhuharov for this contribution!

#### Other Fixes and Dependency Updates

As always, we’ve updated a bunch of dependencies to keep Terrakube up to date and secure, and we’ve also fixed multiple bugs reported by the community. You can see the full list of changes [here](https://github.com/AzBuilder/terrakube/releases/tag/2.22.0).

### May 2024 (2.21.0)

Welcome to the 2.21.0 release from Terrakube! As we reach the midpoint of the year, we’re excited to introduce several new features and enhancements that improve the overall user experience and functionality. Let’s dive into the key highlights:

#### Terrakube Actions

Terrakube actions allow you to extend the UI in Workspaces, functioning similarly to plugins. These actions enable you to add new functions to your Workspace, transforming it into a hub where you can monitor and manage your infrastructure and gain valuable insights. With Terrakube actions, you can quickly observe your infrastructure and apply necessary actions.

In the below demo, you can view how to use a couple of the new actions to [monitor metrics](https://docs.terrakube.io/user-guide/workspaces/actions/built-in-actions/terrakube.azure-monitor) and [restart an Azure VM](https://docs.terrakube.io/user-guide/workspaces/actions/built-in-actions/restart-azure-vm).

![actions](https://github.com/AzBuilder/terrakube/assets/27365102/940862ef-7d20-4bab-9fe7-c76d62e5d589)

Additionally, we are providing an action to interact with OpenAI directly from the workspace. Check out the [Terrakube OpenAI Action](https://docs.terrakube.io/user-guide/workspaces/actions/built-in-actions/terrakube.open-ai) for more details.

![openai](https://github.com/AzBuilder/terrakube/assets/27365102/d5cbf5c8-ce6c-4023-99bc-c8e6ed2eba72)

Check our [documentation](https://docs.terrakube.io/user-guide/workspaces/actions) for more details on how to use and start creating Terrakube actions.

#### Authenticate Providers with Dynamic Credentials

Terrakube's dynamic provider credentials feature allows you to establish a secure trust relationship between Terraform/OpenTofu and your cloud provider. By using unique, short-lived credentials for each job, this feature significantly reduces the risk of compromised credentials. We are excited to announce support for dynamic credentials for the following cloud providers:

* [**AWS**](https://docs.terrakube.io/user-guide/workspaces/dynamic-provider-credentials/aws-dynamic-provider-credentials)
* [**Google Cloud Platform**](https://docs.terrakube.io/user-guide/workspaces/dynamic-provider-credentials/gcp-dynamic-provider-credentials)
* [**Azure**](https://docs.terrakube.io/user-guide/workspaces/dynamic-provider-credentials/azure-dynamic-provider-credentials)

This enhancement ensures that your infrastructure management is both secure and efficient, minimizing potential security vulnerabilities while maintaining seamless integration with your cloud environments.

#### Support for --auto-approve Flag

We are pleased to announce the addition of support for the --auto-approve flag in the CLI-driven workflow. This feature allows for the automatic approval of Terraform and OpenTofu apply operations.

```
user@pop-os:~/git/simple-terraform$ terraform apply --auto-approve

Running apply in Terraform Cloud. Output will stream here. Pressing Ctrl-C
will cancel the remote apply if it's still pending. If the apply started it
will stop streaming the logs, but will not stop the apply running remotely.

Preparing the remote apply...

To view this run in a browser, visit:
https://8080-azbuilder-terrakube-jdlq1j61x7k.ws-us110.gitpod.io/app/simple/auto-approve/runs/1

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

  # time_sleep.wait_30_seconds will be created
  + resource "time_sleep" "wait_30_seconds" {
      + create_duration = (known after apply)
      + id              = (known after apply)
    }

  # module.time_module.random_integer.time will be created
  + resource "random_integer" "time" {
      + id     = (known after apply)
      + max    = 5
      + min    = 1
      + result = (known after apply)
    }

Plan: 3 to add, 0 to change, 0 to destroy.

Changes to Outputs:
  + creation_time = (known after apply)
  + fake_data     = {
      + data     = "Hello World"
      + resource = {
          + resource1 = "fake"
        }
    }

module.time_module.random_integer.time: Creating...
module.time_module.random_integer.time: Creation complete after 0s [id=5]
time_sleep.wait_30_seconds: Creating...
time_sleep.wait_30_seconds: Creation complete after 5s [id=2024-04-16T22:25:15Z]
null_resource.next: Creating...
null_resource.next: Creation complete after 0s [id=7860036023219461249]

Apply complete! Resources: 6 added, 0 changed, 0 destroyed.

Outputs:

creation_time = "5s"
fake_data = {
  "data" = "Hello World"
  "resource" = {
    "resource1" = "fake"
  }
}

```

#### UI Enhancements

We have made several enhancements to the UI, focusing on improving the overall user experience:

* **Buttons:** Basic rounded corner adjustments.
* **Global Shadow Optimization:** Adjusted from three layers of shadows to two layers, used in various components.
* **Loading Spinners:** Added spinners for loading main pages instead of static loading text.
* **Field Validations:** Added validations for fields to improve form submission accuracy.

&#x20;

<figure><img src="https://github.com/AzBuilder/terrakube/assets/27365102/1723b589-121e-47df-9d66-df734b6c548c" alt=""><figcaption></figcaption></figure>

These updates aim to make the Terrakube interface more intuitive and visually appealing.

#### Bug fixes

We have made some improvements and fixes based on the feedback from the community and the security analysis. You can find the full list of changes for this version here https://github.com/AzBuilder/terrakube/releases/tag/2.21.0

### March 2024 (2.20.0)

Welcome to the 2.20.0 release from Terrakube! In our second update of the year, we're excited to share several key highlights with you:

#### Workspace Importer

Our newly Workspace Importer is designed to improve your migration from Terraform Cloud or Terraform Enterprise to Terrakube. Providing a user-friendly wizard, this feature simplifies the process of transferring all your workspaces to Terrakube, ensuring that variables, tags, state files, and other components are easily copied over. During its beta phase, this feature has been tested with hundreds of workspaces, receiving positive feedback from the community. If you're considering a migration to Terrakube, we highly recommend reviewing [our documentation](https://docs.terrakube.io/user-guide/migrating-to-terrakube#migrating-with-the-workspaces-importer) to see how this feature can save you time and effort.

![image](https://github.com/AzBuilder/terrakube/assets/27365102/0d29073f-587a-4e6c-bd49-1355642b1403)

![image](https://github.com/AzBuilder/terrakube/assets/27365102/04918c2b-3ba9-4d8b-8826-918248a1a715)

#### Enhanced API Token Management

We've enhanced the UI for managing [API tokens](https://docs.terrakube.io/user-guide/organizations/api-tokens), making it more intuitive and efficient. Now, revoking tokens can be done directly through the UI with ease.

![image](https://github.com/AzBuilder/terrakube/assets/27365102/1f0bf586-b1d1-45bd-b828-0215a94fa645)

#### Organization Default Execution Mode Update

This new feature allows you to set the default execution mode—either `local` or `remote`—at the organization level.

![image](https://github.com/AzBuilder/terrakube/assets/27365102/f6b74739-bdd9-4534-99aa-67298d9da14f)

#### Enhanced Support for Monorepos in Private Registry

Responding to community feedback regarding the management of Terraform modules within a monorepo, we've made significant enhancements to our Private Registry. Now, you have the capability to specify the path of the module within the monorepo and set a tag prefix when creating a module.

![image](https://github.com/AzBuilder/terrakube/assets/27365102/39c2a211-905c-4540-a9fe-2601928c6525)

#### Agents Support

We've expanded Terrakube capabilities with the introduction of multiple `terrakube executor agents`. Now, you have the flexibility to select the specific agent you wish to use for each workspace. This new feature enables a higher level of execution isolation based on workspaces, offering improved security and customization to suit your project's needs.

![image](https://github.com/AzBuilder/terrakube/assets/27365102/eed1353a-562e-40ba-b470-935b0c73a3a4)

#### Prefix Support for Remote Backend

If you are using the remote backend this enhancement allows you to perform operations across multiple workspaces under the same prefix, improving workspace management and operation execution.

```terraform
terraform {
  backend "remote" {
    hostname = "8080-azbuilder-terrakube-7tnuq3gnkgf.ws-us108.gitpod.io"
    organization = "simple"

    workspaces {
      prefix = "my-app-"
    }
  }
}
```

#### Breaking change for POSTGRESQL user

if you are using POSTGRESQL and using the helm chart 3.16.0 it will require you to add the database port using the property

```
api.properties.databasePort: "5432"
```

More information can be found in this [issue](https://github.com/AzBuilder/terrakube/issues/784)

#### Bug fixes

We have made some improvements and fixes based on the feedback from the community and the security analysis. You can find the full list of changes for this version here https://github.com/AzBuilder/terrakube/releases/tag/2.20.0



### January 2024 (2.19.0)

As we step into the new year, we are thrilled to announce the release of Terrakube 2.19.0, a significant update that continues to push the boundaries of Infrastructure as Code (IaC) management. Here are some key highlights of this release:

#### OpenTofu Now Available on Terrakube

With OpenTofu now in General Availability (GA), we've integrated it as an alternative within Terrakube. Now, when you create workspaces, you have the option to select either Terraform or OpenTofu. Terrakube will then tailor the user experience specifically to the tool you choose. Additionally, you can switch your existing workspaces to OpenTofu via the Workspace settings, allowing for a smooth transition between tools.

![image](https://github.com/AzBuilder/terrakube/assets/27365102/a722d515-ec9e-4dc6-b244-03733009ddc2)

![image](https://github.com/AzBuilder/terrakube/assets/27365102/dca7e280-32d1-4d1c-bb80-ddbdb1398f58)

![image](https://github.com/AzBuilder/terrakube/assets/27365102/b762358c-b23f-4722-bdcc-b9b24301dc4e)

In the future we are planning to introduce specific features for each Terraform and Opentofu based in their evolution. Also we will introduce support for more Iac tools later.

#### Enhanced Workspace Overview Page

We've upgraded the Workspace Overview Page to enrich your interaction with your infrastructure. Now, you'll find a detailed count of resources and outputs. Each resource is represented by an icon indicating its type, with current support for AWS and Azure icons. Clicking on a resource name unfolds a detailed view of all its attributes and dependencies, and provides direct access to the resource's documentation.

![image](https://github.com/AzBuilder/terrakube/assets/27365102/cff434dc-8b9f-46b0-84ef-29b9dacbb4fa)

![image](https://github.com/AzBuilder/terrakube/assets/27365102/3e56f159-e763-4596-801d-46f731f6cd8a)

You can access the attributes and dependencies view from both Workspace Overview Page or using the [Visual State](https://docs.terrakube.io/user-guide/workspaces/terraform-state#visual-state).

#### Webhook Improvements

Now, you have the flexibility to select the default template that will execute for each new push in your Version Control System (VCS) at the workspace level. By default, Terrakube performs Plan and Apply operations, but you can now choose a different template tailored to your specific use case. This enhancement allows for the implementation of customized workflows across different organizations or workspaces.

![image](https://github.com/AzBuilder/terrakube/assets/27365102/9b777ada-959f-4014-a5d7-c550dfed936b)

Additionally, Terrakube now checks for the configured directory in the Workspace and will only run the job when a file changes in the specified directory. This feature is particularly useful for monorepositories, where you want to manage different workspaces from a single repository. This functionality has been implemented for GitLab, GitHub, and Bitbucket.

#### CLI Driven workflow

When leveraging the [CLI-driven workflow](https://docs.terrakube.io/user-guide/workspaces/cli-driven-workflow), you now have the option to utilize the `refresh` flag.

Here's an example without the `refresh` flag:

```
user@pop-os:~/git/simple-terraform$ terraform plan

Running plan in Terraform Cloud. Output will stream here. Pressing Ctrl-C
will stop streaming the logs, but will not stop the plan running remotely.

Preparing the remote plan...

To view this run in a browser, visit:
https://8080-azbuilder-terrakube-3it4oxf0vpt.ws-us107.gitpod.io/app/simple/simple-terraform/runs/2

Waiting for the plan to start...

***************************************
Running Terraform PLAN
***************************************
null_resource.previous: Refreshing state... [id=2942672164070633234]
module.time_module.random_integer.time: Refreshing state... [id=4]
time_sleep.wait_30_seconds: Refreshing state... [id=2023-12-28T20:29:16Z]
null_resource.next: Refreshing state... [id=1447023117451433293]
null_resource.next2: Refreshing state... [id=8910164257527828565]

Terraform used the selected providers to generate the following execution
plan. Resource actions are indicated with the following symbols:
  + create

Terraform will perform the following actions:

  # null_resource.next3 will be created
  + resource "null_resource" "next3" {
      + id = (known after apply)
    }

Plan: 1 to add, 0 to change, 0 to destroy.

```

Example setting `refresh` to false:

```
terraform plan -refresh=false

Running plan in Terraform Cloud. Output will stream here. Pressing Ctrl-C
will stop streaming the logs, but will not stop the plan running remotely.

Preparing the remote plan...

To view this run in a browser, visit:
https://8080-azbuilder-terrakube-3it4oxf0vpt.ws-us107.gitpod.io/app/simple/simple-terraform/runs/3

Waiting for the plan to start...

***************************************
Running Terraform PLAN
***************************************

Terraform used the selected providers to generate the following execution
plan. Resource actions are indicated with the following symbols:
  + create

Terraform will perform the following actions:

  # null_resource.next3 will be created
  + resource "null_resource" "next3" {
      + id = (known after apply)
    }

Plan: 1 to add, 0 to change, 0 to destroy.
```

#### API Token Revocation

You now have the ability to revoke both Personal and Team API Tokens. Currently, this functionality is exclusively accessible via the API, but it will be extended to the UI in a forthcoming release. For more information, please refer to [our documentation](https://docs.terrakube.io/api/methods/personal-access-token-1).

#### Bug fixes

We have made some improvements and fixes based on the feedback from the community and the security analysis. You can find the full list of changes for this version here https://github.com/AzBuilder/terrakube/releases/tag/2.19.0

### December 2023 (2.18.0)

Greetings! As the year comes to a close, we are excited to present you the 2.18.0 release of Terrakube. This version brings you some features and enhancements that we hope you’ll enjoy, some of the key highlights include:

#### AWS icons in State Diagram

The state diagram now includes all the AWS architecture icons, so you can easily recognize each resource type. This feature will help you visualize your Terraform state in a more intuitive way. We are also working on improving the diagram to group the resources by VPC, location, etc which will give you a closer look at how your architecture is designed using the Terraform state.

![image](https://github.com/AzBuilder/terrakube/assets/27365102/c27d9cd2-e261-489e-9c65-df4e99d47c2b)

#### Visual State Diagram export option

Following the state diagram enhancements, we have also added the option to save the diagram in different formats. You can export a high-quality image in png, svg or jpeg formats by using the Download and selecting the preferred format. This feature will help you use the diagram for your documentation or presentations.

![image](https://github.com/AzBuilder/terrakube/assets/27365102/3f5eee59-e4cc-4849-91a5-bbcc51239b32)

#### Self Hosted VCS Providers Support

We have added support for Github Enterprise, GitLab Enterprise Edition and GitLab Community Edition. This means that you can now integrate your workspaces and modules using your self-hosted VCS providers. This is a major milestone in our vision of providing an enterprise-grade experience with Terrakube. In the upcoming releases we will add Bitbucket Server and Azure DevOps Server. For more details in how to use this feature check the following links.

* [Github Enterprise](https://docs.terrakube.io/user-guide/vcs-providers/github-enterprise)

![image](https://github.com/AzBuilder/terrakube/assets/27365102/a341cc42-c4bc-4287-9f77-61d33031a4bc)

* [GitLab Enterprise and Community Editions](https://docs.terrakube.io/user-guide/vcs-providers/gitlab-ee-and-ce)

![image](https://github.com/AzBuilder/terrakube/assets/27365102/1bca2baf-5f18-4e6b-b8a1-bdc0561770ba)

#### Webhooks support for GitLab (Cloud, EE and CE), BitBucket and Github (Cloud and Enterprise)

We have introduced a new feature that will make your workflow more efficient and convenient. Now, Terrakube will automatically trigger a new Plan and Apply job for each new push in the repository that is linked to the workspace. You no longer have to run the jobs manually using the UI or the terraform CLI. This feature will help you keep your infrastructure in sync with your code changes.

![image](https://github.com/AzBuilder/terrakube/assets/27365102/cbee6cbb-531b-488f-8396-a52f65f49e2a)

#### Bug fixes

We have made some improvements and fixes based on the feedback from the community and the security analysis. You can find the full list of changes for this version here https://github.com/AzBuilder/terrakube/releases/tag/2.18.0

### November 2023 (2.17.0)

Welcome to the 2.17.0 release of Terrakube. There are a couple of updates in this version that we hope you'll like, some of the key highlights include:

#### Submodules view

You can now view the submodules of your Terraform Module in the open registry. When you select a submodule, you will see its details, such as inputs, outputs and resources.

![image](https://github.com/AzBuilder/terrakube/assets/27365102/5ef297db-f65c-452f-aea7-bc0ee07a232b)

![image](https://github.com/AzBuilder/terrakube/assets/27365102/6a82dc09-1935-44b3-ae09-fa5797c69a8e)

#### Workspace Overview

We have enhanced the workspace overview to give you more information at a glance. You can now see the Terraform version, the repository, and the list of resources and outputs for each workspace.

![image](https://github.com/AzBuilder/terrakube/assets/27365102/5910b408-b941-4168-967d-d7405fd7541b)

![image](https://github.com/AzBuilder/terrakube/assets/27365102/0567a0f2-3159-4563-b6f9-a160c804302a)

#### Team API Tokens

You can now generate team API tokens using the Terrakube UI and specify their expiration time in days/minutes/hours. This gives you more flexibility and control over your team’s access to Terrakube. To learn more, please visit our [documentation](https://docs.terrakube.io/user-guide/organizations/api-tokens#user-api-tokens-1).

![image](https://github.com/AzBuilder/terrakube/assets/27365102/185b5ec3-c11f-4005-9e57-05d236ca99b3)

![image](https://github.com/AzBuilder/terrakube/assets/27365102/baf010ef-d190-4894-89ab-36f0db4fc033)

### Terraform provider for Terrakube

You can use the [Terrakube provider](https://registry.terraform.io/providers/AzBuilder/terrakube/latest/docs) for Terraform to manage modules, organizations and so on.

```
provider "terrakube" {
  endpoint = "http://terrakube-api.minikube.net"
  token    = "(PERSONAL ACCESS TOKEN OR TEAM TOKEN)"
}

data "terrakube_organization" "org" {
  name = "simple"
}

data "terrakube_vcs" "vcs" {
  name            = "sample_vcs"
  organization_id = data.terrakube_organization.org.id
}

data "terrakube_ssh" "ssh" {
  name            = "sample_ssh"
  organization_id = data.terrakube_organization.org.id
}

resource "terrakube_team" "team" {
  name             = "TERRAKUBE_SUPER_ADMIN"
  organization_id  = data.terrakube_vcs.vcs.organization_id
  manage_workspace = false
  manage_module    = false
  manage_provider  = true
  manage_vcs       = true
  manage_template  = true
}

resource "terrakube_module" "module1" {
  name            = "module_public_connection"
  organization_id = data.terrakube_ssh.ssh.organization_id
  description     = "module_public_connection"
  provider_name   = "aws"
  source          = "https://github.com/terraform-aws-modules/terraform-aws-vpc.git"
}
```

#### Bug fixes

We’ve addressed some issues reported by the community and fixed some vulnerabilities. You can see the full change log for this version here https://github.com/AzBuilder/terrakube/releases/tag/2.17.0

### October 2023 (2.16.0)

Welcome to the 2.16.0 release of Terrakube. There are a couple of updates in this version that we hope you'll like, some of the key highlights include:

#### Improvement in Workspaces List

We have added more filters to workspaces, so you can easily find the one you need. You can now use the tags or description of the workspaces to narrow down your search. Additionally, you can see the tags of each workspace on its card, without having to enter it. This way, you can quickly identify the workspaces that are relevant to you.

![image](https://github.com/AzBuilder/terrakube/assets/27365102/08efcfaa-cf37-4c46-8919-2f34b4e057fc)

#### Team API Tokens

In the previous version, you could only generate API tokens that were associated with a user account. Now, you can also generate API tokens that are linked to a specific team. This gives you more flexibility and control over your API access and permissions. To generate a team API token, you need to use the API for now, but we are working on adding this feature to the UI in the next version. You can find more information on how to use team API tokens in our [documentation](https://docs.terrakube.io/user-guide/organizations/team-tokens)

```
POST {{terrakubeApi}}/access-token/v1/teams
Authorization: Bearer (PERSONAL ACCESS TOKEN)
Request Body:
{
  "days": "1",
  "group": "TERRAKUBE_ADMIN",
  "description": "Sample Team Token"
}
```

#### Bug fixes and Performance

We’ve addressed some issues reported by the community regarding performance,bugs and security. You can see the full change log for this version here https://github.com/AzBuilder/terrakube/releases/tag/2.16.0



### August 2023 (2.15.0)

Welcome to the 2.15.0 release of Terrakube. There are a couple of updates in this version that we hope you'll like, some of the key highlights include:

#### Local Execution Mode

In the previous version, all executions were run remotely in Terrakube. However, starting from this version, you can choose to define the execution mode as either local or remote. By default, all workspaces run remotely. If you want to disable remote execution for a workspace, you can change its execution mode to “Local”. This mode allows you to perform Terraform runs locally using the CLI-driven run workflow.

![image](https://github.com/AzBuilder/terrakube/assets/27365102/d0e40b6c-0830-4153-abe3-3ce43c14e97c)

#### Accessing State from Other Workspaces

Now its possible to use `terraform_remote_state` datasource to acces state between workspaces. For further details check [our documentation](https://docs.terrakube.io/user-guide/workspaces/share-workspace-state)

```
data "terraform_remote_state" "remote_creation_time" {
  backend = "remote"
  config = {
    organization = "simple"
    hostname = "8080-azbuilder-terrakube-vg8s9w8fhaj.ws-us102.gitpod.io"
    workspaces = {
      name = "simple_tag1"
    }
  }
}
```

#### Support Cloud Block with Tags

In our previous version, we introduced support for adding tags to Workspaces. Starting from this version, you can use these tags within the cloud block to apply Terraform operations across multiple workspaces that share the same tags. You can find more details in the [CLI-Driven workflow documentation](https://docs.terrakube.io/user-guide/workspaces/cli-driven-workflow)

```
terraform {
  cloud {
    organization = "simple"
    hostname = "8080-azbuilder-terrakube-md8dylvuzta.ws-us101.gitpod.io"

    workspaces {
      tags = ["networking", "development"]
    }
  }
}
```

#### Search modules

We’ve added a search function to the Modules pages, making it easier to find modules.

#### Bug fixes

We’ve addressed some issues reported by the community, so Terrakube is becoming more stable. You can see the full change log for this version here https://github.com/AzBuilder/terrakube/releases/tag/2.15.0

Thanks for all the community support and contributions. Special thanks to @klinux for implementing the search feature inside Modules.

###

### Jun 2023 (2.14.0)

Welcome to the 2.14.0 release of Terrakube. There are a couple of updates in this version that we hope you'll like, some of the key highlights include:

#### Terraform cloud block support

This new version lets you use the [terraform cloud block](https://developer.hashicorp.com/terraform/cli/cloud/settings#the-cloud-block) to execute remote operations using the [CLI driven run workflow](https://docs.terrakube.io/user-guide/workspaces/cli-driven-workflow).

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

#### Enhanced UI error messages for Authorization errors

We have enhanced the UI to handle authorization errors more gracefully.

![image](https://github.com/AzBuilder/terrakube/assets/27365102/fd556517-e663-42d4-ba46-f20199cb322e)

#### Workspace Tags

We have enabled tagging functionality within the workspaces. In an upcoming release, you will be able to use these tags in the cloud block feature. For more details please check the [terrakube documentation](https://docs.terrakube.io/user-guide/organizations/tags).

![image](https://github.com/AzBuilder/terrakube/assets/27365102/07551e69-a52f-45e8-ba64-cbd8f8acf663)

![image](https://github.com/AzBuilder/terrakube/assets/27365102/78cecea5-503d-4672-a46b-04e48831ea87)

#### Overview Workspace page

With Terrakube’s new workspace Overview page, you can access all the essential information for each of your workspaces in a simple and convenient panel. Initially we are including the last run and workspace tags. We will add more information in upcoming releases.

![image](https://github.com/AzBuilder/terrakube/assets/27365102/7d5fff7d-eed6-48a9-b1bc-8a4f114224f7)

#### Improved CLI run workflow logs

From this version onwards, you can view the logs in real time when you use the Terraform CLI remote backend or the cloud block.

![image](https://github.com/AzBuilder/terrakube/assets/27365102/d3082abe-5ed1-4221-88c9-9eaa13dbe5e8)

#### SSH key injection

Before, Terrakube only used SSH keys to clone private git repos. Now, Terrakube will inject the keys into the workspace. This way, if your terraform modules use git as source, Terraform will access the modules with the keys. Check the [documentation](https://docs.terrakube.io/user-guide/vcs-providers/ssh#ssh-key-injection) for further details.

For the full changelog for this version please check https://github.com/AzBuilder/terrakube/releases/tag/2.14.0



### May 2023 (2.13.0)

Welcome to the 2.13.0 release of Terrakube. There are a couple of updates in this version that we hope you'll like, some of the key highlights include:

#### Terraform init -migrate-state support

Now you can easily migrate your Terraform state from other tools like TFE or Terraform Cloud to Terrakube. The migration command is supported by using the terraform CLI.

```
terraform login 8080-azbuilder-terrakube-q8aleg88vlc.ws-us92.gitpod.io
terraform init -migrate-state
```

Check out our [documentation](https://docs.terrakube.org/user-guide/remote-state-migration) for further details about the process.

#### Improved Log streaming in the UI

Starting with this version, you can see the logs in the Terrakube UI in almost real time, so you can easily follow up on your Terraform changes.

![image](https://github.com/AzBuilder/terrakube/assets/27365102/069d08dd-9ba4-4b1c-b98d-549661a59077)

Also we optimized the code and updated most libs to the latest versions For the full changelog for this version please check https://github.com/AzBuilder/terrakube/releases/tag/2.13.0



### Mar 2023 (2.12.0)

Welcome to the 2.12.0 release of Terrakube. There are a couple of updates in this version that we hope you'll like, some of the key highlights include:

#### Search options in Workspaces

Use the new controls in the workspaces list to filter by status or search by name.

![image](https://user-images.githubusercontent.com/27365102/227623916-7d6633bf-835f-4b6e-8aba-0f35fd31dc50.png)

#### Simplified process to add a new VCS provider

You can now connect to a vcs provider in one step, instead of creating it first and then updating the call back url.

![image](https://user-images.githubusercontent.com/27365102/227625007-37fa43e1-9946-4b05-9a0a-2eb02006d682.png)

#### Open Telemetry

We added Open Telemetry to the api, registry and executor components for monitoring Terrakube. In future versions, we will also provide Grafana templates for custom metrics like jobs execution and workspaces usage. Check the [documentation](https://docs.terrakube.org/getting-started/deployment/open-telemetry) for further information.

![image](https://user-images.githubusercontent.com/4461895/223861732-82bf163b-f3c8-4463-af51-3953c7ae9f76.png)

#### Improvement to Templates

* In the previous version we introduced the use of `importCommands`, and now improve reutilization we are adding `inputsTerraform` and `inputsEnv` so basically now you can send parameters to your templates in an easy way. For example:

```yaml
flow:
  - type: "terraformPlan"
    step: 100
    importComands:
      repository: "https://github.com/AzBuilder/terrakube-extensions"
      folder: "templates/terratag"
      branch: "import-template"
      inputsEnv:
        INPUT_IMPORT1: $IMPORT1
        INPUT_IMPORT2: $IMPORT2
      inputsTerraform:
        INPUT_IMPORT1: $IMPORT1
        INPUT_IMPORT2: $IMPORT2
  - type: "terraformApply"
    step: 200
```

Check the [documentation](https://docs.terrakube.org/user-guide/organizations/templates/filter-gloval-variables-in-jobs) for more details.

* And if you want to create a schedule for a template, you can do it easily now with `scheduleTemplates`.

```yaml
flow:
  - type: "customScripts"
    step: 100
    inputsTerraform:
      SIZE: $SIZE_SMALL
    commands:
      - runtime: "BASH"
        priority: 100
        before: true
        script: |
          echo "SIZE : $SIZE"
  - type: "scheduleTemplates"
    step: 200
    templates:
      - name: "schedule1"
        schedule: "0 0/5 * ? * * *"
      - name: "schedule2"
        schedule: "0 0/10 * ? * * *"
```

Check [here](https://docs.terrakube.org/user-guide/organizations/templates/template-scheduling-in-jobs) for more details.

#### Restrict Terraform Versions or Provide your own Terraform build

You can now use a custom Terraform CLI build or restrict the Terraform versions in your organization with terrakube. Check the [documentation](https://docs.terrakube.org/getting-started/deployment/custom-terraform-cli-builds) for more details.

#### Terrakube Installation

We improved the helm chart to make the installation easy and now you can quickly install Terrakube for a Sandbox Test environment in [Minikube](https://docs.terrakube.org/getting-started/deployment/minikube). If you prefer you can now try Terrakube using [Docker compose](https://docs.terrakube.org/getting-started/docker-compose)

Thanks for the community contribution, specially thanks to @Diliz and @jstewart612 for their contributions in the Helm Chart. For the full changelog for this version please check https://github.com/AzBuilder/terrakube/releases/tag/2.12.0

And if you have any idea or suggestion don't hesitate to let us know.



### February 2023 (2.11.0)

Welcome to the 2.11.0 release of Terrakube. There are a couple of updates in this version that we hope you'll like, some of the key highlights include:

#### Adding Global Variables to Organization Settings

Global Variables allow you to define and apply variables one time across all workspaces within an organization. So you can set your cloud provider credentials and reuse it in all the workspaces inside the same organization. See more details in [our documentation](https://docs.terrakube.org/user-guide/organizations/global-variables)

![image](https://user-images.githubusercontent.com/27365102/221226933-ee19e11e-ed97-448f-8213-5c4848d5289b.png)

#### Adding CLI and API Driven workflows to workspaces

[CLI-driven](https://docs.terrakube.org/user-guide/workspaces/creating-workspaces#cli-driven-workflow) and [API-driven](https://docs.terrakube.org/user-guide/workspaces/creating-workspaces#cli-driven-workflow) workflows are available now. This mean you can create a workspace in the UI and then connect to the same using the Terraform cli or the Terrakube API. Or if you prefer you can go ahead and connect to your Terrakube organization locally from the Terraform cli. Check the [CLI-driven worflow documentation](https://docs.terrakube.org/user-guide/workspaces/cli-driven-workflow) for further details.

For CLI or API driven workspaces you will see the steps directly in the overview tab:

![image](https://user-images.githubusercontent.com/27365102/221228244-89f6993c-2fbb-481b-ad45-b0520e5f2fb9.png)

And in the list of workspaces you will see a badge so you can easily identify CLI and API driven workspaces

![image](https://user-images.githubusercontent.com/27365102/221229865-885cf590-706b-4a3f-bef2-fb14707570aa.png)

#### Improved error handling for invalid templates

If you have an error in your template yaml syntax will be easy to identify it. You will see a more clear message about the job execution failure so you can easily see if the error is related with the execution or with the template configuration.

![image](https://user-images.githubusercontent.com/27365102/221230638-d65335cd-29c5-4454-b6cf-4b9665b4cc1e.png)

#### Import scripts inside the templates

If you have some scripts that you want to reuse, you can import it using the new `importCommand` this will improve the reuse of templates, so you can share some commons steps with the community in your github repos. See [Terrakube docs](https://docs.terrakube.org/user-guide/organizations/templates/import-templates) for more details in how to import scripts

```yaml
flow:
  - type: "terraformPlan"
    step: 100
    importComands:
      repository: "https://github.com/AzBuilder/terrakube-extensions"
      folder: "templates/terratag"
      branch: "import-template"
  - type: "terraformApply"
    step: 200
```

#### Adding UI templates support in workspaces

UI templates extend the idea of templates to the UI, so you can customize how each step will look like, in your custom flow inside Terrakube. Basically you can use standard HTML to build the way in which Terrakube will present the specific step

For example you can present a more user friendly information when extracting data from infracost result. And in the UI if you have a custom Template for that step you will se the Structured UI by default, but you can switch the view to the standard terminal output to see the full logs for that step.

Check some examples:

![image](https://user-images.githubusercontent.com/27365102/221233136-f1efde52-88db-4407-af41-fab66d8db460.png) ![image](https://user-images.githubusercontent.com/27365102/221233363-d23d52d9-269c-4274-872c-03c2f204a2ef.png)

UI templates will allow to create better UI experiences. And brings new ways to customize the way you use Terrakube in your organization. In the upcoming releases we will create some standards templates that you can reuse for the terraform plan and apply. And also we will provide a Terrakube extensions that allows to create UI templates easily.

#### Improving organization selection on initial login

When you login to Terrakube for the first time and if you have multiple organizations assigned, then you will see a screen to select your organization.

For the full changelog for this version please check https://github.com/AzBuilder/terrakube/releases/tag/2.11.0

And if you have any idea or suggestion don't hesitate to let us know.
