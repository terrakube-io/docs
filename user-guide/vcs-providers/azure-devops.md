# Azure DevOps

For using repositories from Azure Devops with Terrakube workspaces and modules you will need to follow these steps:

{% hint style="info" %}
**Manage VCS Providers** permission is required to perform this action, please check [team-management.md](../organizations/team-management.md "mention") for more info.
{% endhint %}

Navigate to the desired organization **** and click the **Settings** button, then on the left menu select **VCS Providers**&#x20;

<figure><img src="../../.gitbook/assets/image (14) (2).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
If you prefer, you can add a new VCS Provider directly from the Create workspace or Create Module screen.
{% endhint %}

Click the **Azure Devops** button

<figure><img src="../../.gitbook/assets/image (11).png" alt=""><figcaption></figcaption></figure>

In the next screen click the link to [register a new OAuth Application](https://aex.dev.azure.com/app/register?mkt=en-US) in Azure Devops

<figure><img src="../../.gitbook/assets/image (6).png" alt=""><figcaption></figcaption></figure>

In the Azure Devops page, complete the required fields and click **Create application**

| Field                          | Description                                                       |
| ------------------------------ | ----------------------------------------------------------------- |
| Company Name                   | Your company name.                                                |
| Application name               | The name of your application or you can use the Organization name |
| Application website            | Your application or website url                                   |
| Callback URL                   | Use https://localhost we will update this value later             |
| Authorized scopes (checkboxes) | Only the following should be checked: Code (read) Code (status)   |

{% hint style="info" %}
You can complete the fields using the information suggested by terrakube in the VCS provider screen
{% endhint %}



<figure><img src="../../.gitbook/assets/image (8).png" alt=""><figcaption></figcaption></figure>

In the next screen, copy the **App ID** and **Client Secret**

<figure><img src="../../.gitbook/assets/image (9).png" alt=""><figcaption></figcaption></figure>

Go back to Terrakube to enter the information you copied from the previous step. Then, click the **Connect and Continue** button.

<figure><img src="../../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

In the next screen copy the **Callback URL,** we will need this value to update our Azure Devops application

<figure><img src="../../.gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>

Go back to Azure Devops and update the Callback URL, click the **Save** button

<figure><img src="../../.gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>

Once you updated the Callback Url in Azure Devops, return to Terrakube and click the **Connect to Azure Devops** button

<figure><img src="../../.gitbook/assets/image (10).png" alt=""><figcaption></figcaption></figure>

You will see a new window, click the **Accept** button to complete the connection

<figure><img src="../../.gitbook/assets/image (12).png" alt=""><figcaption></figcaption></figure>

Finally if the connection was stablished successfully, you wil see a **Connected** message, you can close the window and return to Terrakube.

<figure><img src="../../.gitbook/assets/image (6) (1) (5).png" alt=""><figcaption></figcaption></figure>

If you refresh the VCS providers page in your organization, you should see the connection status with the date and the user that created the connection

<figure><img src="../../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

And now, you will be able to use the connection in your workspaces and modules:

<figure><img src="../../.gitbook/assets/image (7).png" alt=""><figcaption></figcaption></figure>
