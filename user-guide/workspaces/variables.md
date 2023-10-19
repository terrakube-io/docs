# Variables

Terrakube workspace variables let you customize configurations and store information like static provider credentials. You can also reference this variables inside your Templates.

You can set [variables specifically for each workspace](variables.md#workspace-specific-variables) or you can create [global-variables.md](../organizations/global-variables.md "mention") to reuse the same variables across multiple workspaces.

### Workspace-Specific Variables <a href="#workspace-specific-variables" id="workspace-specific-variables"></a>

To view and manage a workspace's variables, go to the workspace and click the **Variables** tab.

The **Variables** page appears, showing all workspace-specific variables. This is where you can add, edit, and delete workspace-specific variables.&#x20;

There are 2 kinds of Variables:

* [Terraform Variables](variables.md#terraform-variables)
* [Environment Variables](variables.md#environment-variables)

### Terraform Variables

These Terraform variables are set using a terraform.tfvars file. To use interpolation or set a non-string value for a variable, click its HCL checkbox.

#### Add a Terraform Variable <a href="#add-a-variable" id="add-a-variable"></a>

Go to the workspace **Variables** page and in the _Terraform Variables_ section click the **Add variable** button.

<figure><img src="../../.gitbook/assets/image (5) (7).png" alt=""><figcaption></figcaption></figure>

In the popup, provide the required values. Use the below table as reference:

| Field       | Description                                                                                                                               |
| ----------- | ----------------------------------------------------------------------------------------------------------------------------------------- |
| Key         | Unique variable name                                                                                                                      |
| Value       | Key value                                                                                                                                 |
| Description | Free text to document the reason for this global variable                                                                                 |
| HCL         | Parse this field as HashiCorp Configuration Language (HCL). This allows you to interpolate values at runtime.                             |
| Sensitive   | Sensitive variables are never shown in the UI or API. They may appear in Terraform logs if your configuration is designed to output them. |

Finally click the **Save variable** button and the variable will be created.

<figure><img src="../../.gitbook/assets/image (7) (2).png" alt=""><figcaption></figcaption></figure>

#### Edit a Terraform Variable

Click the **Edit** button next to the variable you want to edit.

<figure><img src="../../.gitbook/assets/image (6) (1).png" alt=""><figcaption></figcaption></figure>

Make any desired changes and click **Save variable.**

#### Delete a Terraform Variable

Click the **Delete** button next to the variable you want to delete.

<figure><img src="../../.gitbook/assets/image (2) (1) (5).png" alt=""><figcaption></figcaption></figure>

Click **Yes** to confirm your action.

<figure><img src="../../.gitbook/assets/image (3) (2).png" alt=""><figcaption></figcaption></figure>

### Environment Variables

These variables are set in Terraform's shell environment using export.

#### Add an Environment Variable <a href="#add-a-variable" id="add-a-variable"></a>

Go to the workspace **Variables** page and in the _Environment Variables_ section click the **Add variable** button.

<figure><img src="../../.gitbook/assets/image (8) (1).png" alt=""><figcaption></figcaption></figure>

In the popup, provide the required values. Use the below table as reference:

| Field       | Description                                                                                                                               |
| ----------- | ----------------------------------------------------------------------------------------------------------------------------------------- |
| Key         | Unique variable name                                                                                                                      |
| Value       | Key value                                                                                                                                 |
| Description | Free text to document the reason for this global variable                                                                                 |
| HCL         | Parse this field as HashiCorp Configuration Language (HCL). This allows you to interpolate values at runtime.                             |
| Sensitive   | Sensitive variables are never shown in the UI or API. They may appear in Terraform logs if your configuration is designed to output them. |

Finally click the **Save variable** button and the variable will be created.

<figure><img src="../../.gitbook/assets/image (1) (1) (1) (1) (2).png" alt=""><figcaption></figcaption></figure>

#### Edit an Environment Variable

Click the **Edit** button next to the variable you want to edit.

<figure><img src="../../.gitbook/assets/image (4) (1).png" alt=""><figcaption></figcaption></figure>

Make any desired changes and click **Save variable.**

#### Delete an Environment Variable

Click the **Delete** button next to the variable you want to delete.

<figure><img src="../../.gitbook/assets/image (3) (1) (3).png" alt=""><figcaption></figcaption></figure>

Click **Yes** to confirm your action.

