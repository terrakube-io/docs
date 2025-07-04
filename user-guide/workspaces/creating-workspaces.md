# Creating Workspaces

{% hint style="info" %}
**Manage Workspaces** permission is required to perform this action, please check [team-management.md](../organizations/team-management.md "mention") for more info.
{% endhint %}

When creating a Workspace, Terrakube supports 3 workflows types and based on the selected workflow you will need to provide some parameters. Please refer to each workflow section for more reference.

* [Version Control workflow](creating-workspaces.md#version-control-workflow): Store your Terraform configuration in a git repository, and trigger runs based on pull requests and merges.
* [CLI-driven workflow](creating-workspaces.md#cli-driven-workflow): Trigger remote Terraform runs from your local command line.
* [API-driven workflow](creating-workspaces.md#api-driven-workflow): A more advanced option. Integrate Terraform into a larger pipeline using the [Terrakube API](../../api/getting-started.md).

### Version Control workflow

Click **Workspaces** in the main menu and then click the **New workspace** button

<figure><img src="../../.gitbook/assets/image (297).png" alt=""><figcaption></figcaption></figure>

Select the IaC type for now we support terraform and open tofu

<figure><img src="../../.gitbook/assets/image (65).png" alt=""><figcaption></figcaption></figure>

Choose the **Version control workflow**

<figure><img src="../../.gitbook/assets/image (319).png" alt=""><figcaption></figcaption></figure>

Select an existing version control provider or click **Connect to a different VCS** to configure a new one. See [vcs-providers](../vcs-providers/ "mention") for more details.

<figure><img src="../../.gitbook/assets/image (294).png" alt=""><figcaption></figcaption></figure>

Provide the git repository URL and click the **Continue** button.

{% hint style="info" %}
If you want to connect to a private git repo using SSH Keys you will need to provide the url in ssh format. Example git@github.com:jcanizalez/terraform-sample-repository.git. For more information see [ssh.md](../vcs-providers/ssh.md "mention")
{% endhint %}

<figure><img src="../../.gitbook/assets/image (317).png" alt=""><figcaption></figcaption></figure>

Configure the workspace settings.

| Field                       | Description                                                                                                                                                                                                                            |
| --------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Workspace Name              | The name of your workspace is unique and used in tools, routing, and UI. Dashes, underscores, and alphanumeric characters are permitted.                                                                                               |
| VCS branch                  | A list of branches separated by comma that jobs are allowed to kicked off from VCS webhook. This is not used for CLI workflow.                                                                                                         |
| Terraform Working Directory | Default workspace directory. Use / for the root folder                                                                                                                                                                                 |
| Terraform Version           | The version of Terraform to use for this workspace. Check [custom-terraform-cli-builds.md](../../getting-started/deployment/custom-terraform-cli-builds.md "mention") if you want to restrict the versions in your Terrakube instance. |

Once you fill the settings click the **Create Workspace** button.

<figure><img src="../../.gitbook/assets/workspace-settings-vcs.png" alt=""><figcaption></figcaption></figure>

When using a VCS workflow you can select which branches, folder and action (aka, Default template (VCS push)) the runs will be triggered from when a git push action happens.

The `VCS branches` accepts a list of branches separated by comma, the first in the list is used as the default for the runs kicked off from UI. The branches are compared as prefixes against the branch included in the payload sent from VCS provider.

<figure><img src="../../.gitbook/assets/image (63).png" alt=""><figcaption></figcaption></figure>

You will be redirected to the Workspace page

<figure><img src="../../.gitbook/assets/image (184).png" alt=""><figcaption></figcaption></figure>

And if you navigate to the **Workspace** menu, you will see the workspace in the Workspaces list

<figure><img src="../../.gitbook/assets/image (299).png" alt=""><figcaption></figcaption></figure>

Once you create your workspace, Terrakube sets up a webhook with your VCS. This webhook runs a job based on the selected template every time you push new changes to the set workspace branches. However, this feature does not work yet with Azure DevOps VCS provider.

### CLI-driven Workflow

Click **Workspaces** in the main menu and then click the **New workspace** button

<figure><img src="../../.gitbook/assets/image (303).png" alt=""><figcaption></figcaption></figure>

Choose the **CLI-driven workflow**

<figure><img src="../../.gitbook/assets/image (316).png" alt=""><figcaption></figcaption></figure>

Configure the workspace settings.

| Field             | Description                                                                                                                                                                                                                            |
| ----------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Workspace Name    | The name of your workspace is unique and used in tools, routing, and UI. Dashes, underscores, and alphanumeric characters are permitted.                                                                                               |
| Terraform Version | The version of Terraform to use for this workspace. Check [custom-terraform-cli-builds.md](../../getting-started/deployment/custom-terraform-cli-builds.md "mention") if you want to restrict the versions in your Terrakube instance. |

Once you fill the settings click the **Create Workspace** button.

<figure><img src="../../.gitbook/assets/image (174).png" alt=""><figcaption></figcaption></figure>

You will be redirected to the Workspace page.

{% hint style="info" %}
The overview page for CLI-driven workspaces show the step to connect to the workspace using the Terraform CLI. For more details see [cli-driven-workflow.md](cli-driven-workflow.md "mention")
{% endhint %}

<figure><img src="../../.gitbook/assets/image (258).png" alt=""><figcaption></figcaption></figure>

And if you navigate to the **Workspace** menu you will see the workspace in the Workspaces list

<figure><img src="../../.gitbook/assets/image (293).png" alt=""><figcaption></figcaption></figure>

### API-driven Workflow

Click **Workspaces** in the main menu and then click the **New workspace** button

<figure><img src="../../.gitbook/assets/image (297).png" alt=""><figcaption></figcaption></figure>

Choose the **API-driven workflow**

<figure><img src="../../.gitbook/assets/image (208).png" alt=""><figcaption></figcaption></figure>

Configure the workspace settings.

| Field             | Description                                                                                                                                                                                                                            |
| ----------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Workspace Name    | The name of your workspace is unique and used in tools, routing, and UI. Dashes, underscores, and alphanumeric characters are permitted.                                                                                               |
| Terraform Version | The version of Terraform to use for this workspace. Check [custom-terraform-cli-builds.md](../../getting-started/deployment/custom-terraform-cli-builds.md "mention") if you want to restrict the versions in your Terrakube instance. |

Once you fill the settings click the **Create Workspace** button.

<figure><img src="../../.gitbook/assets/image (218).png" alt=""><figcaption></figcaption></figure>

ou will be redirected to the Workspace page.

{% hint style="info" %}
For more details how to use the Terrakube API. See [api-driven-workflow.md](api-driven-workflow.md "mention")
{% endhint %}

<figure><img src="../../.gitbook/assets/image (291).png" alt=""><figcaption></figcaption></figure>

And if you navigate to the **Workspace** menu you will see the workspace in the Workspaces list

<figure><img src="../../.gitbook/assets/image (162).png" alt=""><figcaption></figcaption></figure>
