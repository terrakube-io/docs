# GitLab

For using repositories from Gitlab.com with Terrakube workspaces and modules you will need to follow these steps:

{% hint style="info" %}
**Manage VCS Providers** permission is required to perform this action, please check [team-management.md](../organizations/team-management.md "mention") for more info.
{% endhint %}



Navigate to the desired organization **** and click the **Settings** button, then on the left menu select **VCS Providers**&#x20;

<figure><img src="../../.gitbook/assets/image (14) (2).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
If you prefer, you can add a new VCS Provider directly from the [Create workspace](../workspaces/creating-workspaces.md) or Create Module screen.&#x20;
{% endhint %}

Click the **Gitlab** button

<figure><img src="../../.gitbook/assets/image (4) (1) (3).png" alt=""><figcaption></figcaption></figure>

In the next screen click the link to [register a new OAuth Application](https://gitlab.com/-/profile/applications) in Gitlab

<figure><img src="../../.gitbook/assets/image (2) (1).png" alt=""><figcaption></figcaption></figure>

On Gitlab, complete the required fields and click **Save application** button

| Field        | Description                                                            |
| ------------ | ---------------------------------------------------------------------- |
| Name         | Your application name, for example you can use your organization name. |
| Redirect URI | Copy the Redirect URI from the UI                                      |
| Scopes       | Only **api** should be checked                                         |

{% hint style="info" %}
You can complete the fields using the information suggested by terrakube in the VCS provider screen
{% endhint %}

<figure><img src="../../.gitbook/assets/image (3) (1).png" alt=""><figcaption></figcaption></figure>

In the next screen, copy the **Application ID** and **Secret**

<figure><img src="../../.gitbook/assets/image (2) (1) (3) (2).png" alt=""><figcaption></figcaption></figure>

Go back to Terrakube to enter the information you copied from the previous step. Then, click the **Connect and Continue** button.

<figure><img src="../../.gitbook/assets/image (7) (1).png" alt=""><figcaption></figcaption></figure>

You will see a Gitlab window, click the **Authorize** button to complete the connection

<figure><img src="../../.gitbook/assets/image (14) (3).png" alt=""><figcaption></figcaption></figure>

Finally if the connection was stablished successfully, you wil see a **Connected** message, you can close the window and return to Terrakube.

<figure><img src="../../.gitbook/assets/image (57).png" alt=""><figcaption></figcaption></figure>

If you navigate to the VCS providers page in your organization, you should see the connection status with the date and the user that created the connection

<figure><img src="../../.gitbook/assets/image (13) (4).png" alt=""><figcaption></figcaption></figure>

And now, you will be able to use the connection in your workspaces and modules:

<figure><img src="../../.gitbook/assets/image (11) (3).png" alt=""><figcaption></figcaption></figure>
