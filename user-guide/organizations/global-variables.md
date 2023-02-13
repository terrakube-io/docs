# Global Variables

Global Variables allow you to define and apply variables one time across all workspaces within an organization. For example, you could define a global variable of provider credentials and automatically apply it to all workspaces.

{% hint style="warning" %}
Workspace variables have priority over global variables if the same name is used.
{% endhint %}

### Creating a Global Variable

{% hint style="info" %}
Only users that belongs to Terrakube administrator group can create global variables. This group is defined in the terrakube settings during deployment, for more details see [#administrator-group](../../getting-started/security.md#administrator-group "mention")
{% endhint %}

Once you are in the desired organization, click the **Settings** button, then in the left menu select the **Global Variables** option and click the **Add global variable** button

<figure><img src="../../.gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>

In the popup, provide the required values. Use the below table as reference:

| Field       | Description                                                                                                                               |
| ----------- | ----------------------------------------------------------------------------------------------------------------------------------------- |
| Key         | Unique variable name                                                                                                                      |
| Value       | Key value                                                                                                                                 |
| Category    | Category could be Terraform Variable or Environment Variable                                                                              |
| Description | Free text to document the reason for this global variable                                                                                 |
| HCL         | Parse this field as HashiCorp Configuration Language (HCL). This allows you to interpolate values at runtime.                             |
| Sensitive   | Sensitive variables are never shown in the UI or API. They may appear in Terraform logs if your configuration is designed to output them. |

Finally click the **Save global variable** button and the variable will be created

<figure><img src="../../.gitbook/assets/image (4) (1).png" alt=""><figcaption></figcaption></figure>

You will see the new global variable in the list. And now the variable will be injected in all the workspaces within the organization

<figure><img src="../../.gitbook/assets/image (8).png" alt=""><figcaption></figcaption></figure>

### Edit a Global Variable

Click the **Edit** button next to the global variable you want to edit.

<figure><img src="../../.gitbook/assets/image (1) (6).png" alt=""><figcaption></figcaption></figure>

Change the fields you need and click the **Save global variable** button

{% hint style="warning" %}
For security, you can't change the **Sensitive** field. So if you want to change one global variable to sensitive you must delete the existing variable and create a new one
{% endhint %}

<figure><img src="../../.gitbook/assets/image (2) (5).png" alt=""><figcaption></figcaption></figure>

### Delete a Global Variable

Click the **Delete** button next to the global variable you want to delete, and then click the Yes button to confirm the deletion. Please take in consideration the deletion is irreversible

<figure><img src="../../.gitbook/assets/image (10).png" alt=""><figcaption></figcaption></figure>
