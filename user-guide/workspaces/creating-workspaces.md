# Creating Workspaces



{% hint style="info" %}
**Manage Workspaces** permission is required to perform this action, please check [team-management.md](../organizations/team-management.md "mention") for more info.
{% endhint %}



When creating a Workspace, Terrakube supports 3 workflows types and based on the selected workflow you will need to provide some parameters. Please refer to each workflow section for more reference.&#x20;

* [Version Control workflow](creating-workspaces.md#version-control-workflow): Store your Terraform configuration in a git repository, and trigger runs based on pull requests and merges.
* [CLI-driven workflow](creating-workspaces.md#cli-driven-workflow): Trigger remote Terraform runs from your local command line.
* [API-driven workflow](creating-workspaces.md#api-driven-workflow): A more advanced option. Integrate Terraform into a larger pipeline using the [Terrakube API](../../api/getting-started.md).

### Version Control workflow

Click **Workspaces** in the main menu and then click the **New workspace** button

<figure><img src="../../.gitbook/assets/image (4) (7).png" alt=""><figcaption></figcaption></figure>

Choose the **Version control workflow**

<figure><img src="../../.gitbook/assets/image (14).png" alt=""><figcaption></figcaption></figure>

Select an existing version control provider or click **Connect to a different VCS** to configure a new one. See [vcs-providers](../vcs-providers/ "mention") for more details.

<figure><img src="../../.gitbook/assets/image (7) (4).png" alt=""><figcaption></figcaption></figure>

Provide the git repository URL and click the **Continue** button.

{% hint style="info" %}
If you want to connect to a private git repo using SSH Keys you will need to provide the url in ssh format. Example git@github.com:jcanizalez/terraform-sample-repository.git. For more information see [ssh.md](../vcs-providers/ssh.md "mention")
{% endhint %}

<figure><img src="../../.gitbook/assets/image (12) (2).png" alt=""><figcaption></figcaption></figure>

Configure the workspace settings.&#x20;

| Field                       | Description                                                                                                                              |
| --------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------- |
| Workspace Name              | The name of your workspace is unique and used in tools, routing, and UI. Dashes, underscores, and alphanumeric characters are permitted. |
| VCS branch                  | The branch from which to import new versions.                                                                                            |
| Terraform Working Directory | Default workspace directory. Use / for the root folder                                                                                   |
| Terraform Version           | The version of Terraform to use for this workspace                                                                                       |

Once you fill the settings click the **Create Workspace** button.

<figure><img src="../../.gitbook/assets/image (1) (2).png" alt=""><figcaption></figcaption></figure>

You will be redirected to the Workspace page

<figure><img src="../../.gitbook/assets/image (1) (1) (4).png" alt=""><figcaption></figcaption></figure>

And if you navigate to the **Workspace** menu, you will see the workspace in the Workspaces list

<figure><img src="../../.gitbook/assets/image (17) (1).png" alt=""><figcaption></figcaption></figure>

### CLI-driven Workflow

Click **Workspaces** in the main menu and then click the **New workspace** button

<figure><img src="../../.gitbook/assets/image (2) (1) (1) (2).png" alt=""><figcaption></figcaption></figure>

Choose the **CLI-driven workflow**

<figure><img src="../../.gitbook/assets/image (5) (6).png" alt=""><figcaption></figcaption></figure>

Configure the workspace settings.&#x20;

| Field             | Description                                                                                                                              |
| ----------------- | ---------------------------------------------------------------------------------------------------------------------------------------- |
| Workspace Name    | The name of your workspace is unique and used in tools, routing, and UI. Dashes, underscores, and alphanumeric characters are permitted. |
| Terraform Version | The version of Terraform to use for this workspace                                                                                       |

Once you fill the settings click the **Create Workspace** button.

<figure><img src="../../.gitbook/assets/image (11) (1).png" alt=""><figcaption></figcaption></figure>

You will be redirected to the Workspace page.

{% hint style="info" %}
The overview page for CLI-driven workspaces show the step to connect to the workspace using the Terraform CLI. For more details see [cli-driven-workflow.md](cli-driven-workflow.md "mention")
{% endhint %}

<figure><img src="../../.gitbook/assets/image (18) (1).png" alt=""><figcaption></figcaption></figure>

And if you navigate to the **Workspace** menu you will see the workspace in the Workspaces list

<figure><img src="../../.gitbook/assets/image (20).png" alt=""><figcaption></figcaption></figure>

### API-driven Workflow

Click **Workspaces** in the main menu and then click the **New workspace** button

<figure><img src="../../.gitbook/assets/image (4) (7).png" alt=""><figcaption></figcaption></figure>

Choose the **API-driven workflow**

<figure><img src="../../.gitbook/assets/image (8) (1) (2).png" alt=""><figcaption></figcaption></figure>

Configure the workspace settings.&#x20;

| Field             | Description                                                                                                                              |
| ----------------- | ---------------------------------------------------------------------------------------------------------------------------------------- |
| Workspace Name    | The name of your workspace is unique and used in tools, routing, and UI. Dashes, underscores, and alphanumeric characters are permitted. |
| Terraform Version | The version of Terraform to use for this workspace                                                                                       |

Once you fill the settings click the **Create Workspace** button.

<figure><img src="../../.gitbook/assets/image (19).png" alt=""><figcaption></figcaption></figure>

ou will be redirected to the Workspace page.

{% hint style="info" %}
For more details how to use the Terrakube API. See [api-driven-workflow.md](api-driven-workflow.md "mention")
{% endhint %}

<figure><img src="../../.gitbook/assets/image (15).png" alt=""><figcaption></figcaption></figure>

And if you navigate to the **Workspace** menu you will see the workspace in the Workspaces list

<figure><img src="../../.gitbook/assets/image (3) (6).png" alt=""><figcaption></figcaption></figure>
