# Azure DevOps

For using repositories from Azure Devops with Terrakube workspaces and modules you will need to follow these steps:

{% hint style="info" %}
**Manage VCS Providers** permission is required to perform this action, please check [team-management.md](../organizations/team-management.md "mention") for more info.
{% endhint %}

Navigate to the desired organization and click the **Settings** button, then on the left menu select **VCS Providers**&#x20;

<figure><img src="../../.gitbook/assets/image (14) (2) (1).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
If you prefer, you can add a new VCS Provider directly from the [Create workspace](../workspaces/creating-workspaces.md) or Create Module screen.
{% endhint %}

Click the **Azure Devops** button

<figure><img src="../../.gitbook/assets/image (11) (2).png" alt=""><figcaption></figcaption></figure>

In the next screen click the link to [register a new OAuth Application](https://aex.dev.azure.com/app/register?mkt=en-US) in Azure Devops

<figure><img src="../../.gitbook/assets/image (33).png" alt=""><figcaption></figcaption></figure>

In the Azure Devops page, complete the required fields and click **Create application**

<table><thead><tr><th width="354">Field</th><th>Description</th></tr></thead><tbody><tr><td>Company Name </td><td>Your company name.</td></tr><tr><td>Application name</td><td>The name of your application or you can use the Organization name</td></tr><tr><td>Application website</td><td>Your application or website url </td></tr><tr><td>Callback URL</td><td>Copy the Callback URL from the Terrakube URL</td></tr><tr><td>Authorized scopes (checkboxes)</td><td>Only the following should be checked: Code (read) Code (status)</td></tr></tbody></table>

{% hint style="info" %}
You can complete the fields using the information suggested by terrakube in the VCS provider screen
{% endhint %}

<figure><img src="../../.gitbook/assets/image (22).png" alt=""><figcaption></figcaption></figure>

In the next screen, copy the **App ID** and **Client Secret**

<figure><img src="../../.gitbook/assets/image (32).png" alt=""><figcaption></figcaption></figure>

Go back to Terrakube to enter the information you copied from the previous step. Then, click the **Connect and Continue** button.

<figure><img src="../../.gitbook/assets/image (24).png" alt=""><figcaption></figcaption></figure>

You will see an Azure Devops window, click the **Accept** button to complete the connection

<figure><img src="../../.gitbook/assets/image (12) (1) (2).png" alt=""><figcaption></figcaption></figure>

Finally, if the connection was established successfully, you will be redirected to the VCS provider’s page in your organization. You should see the connection status with the date and the user that created the connection.

<figure><img src="../../.gitbook/assets/image (23).png" alt=""><figcaption></figcaption></figure>

And now, you will be able to use the connection in your workspaces and modules:

<figure><img src="../../.gitbook/assets/image (7) (1) (1).png" alt=""><figcaption></figcaption></figure>
