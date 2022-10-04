# Github

For using repositories from GitHub.com with Terrakube workspaces and modules you will need to follow these steps:

{% hint style="info" %}
**Manage VCS Providers** permission is required to perform this action, please check [team-management.md](../organizations/team-management.md "mention") for more info.
{% endhint %}

Navigate to the desired organization **** and click the **Settings** button, then on the left menu select **VCS Providers**&#x20;

<figure><img src="../../.gitbook/assets/image (14) (2).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
If you prefer, you can add a new VCS Provider directly from the Create workspace or Create Module screen.&#x20;
{% endhint %}

Click the **Github button**

<figure><img src="../../.gitbook/assets/image (7).png" alt=""><figcaption></figcaption></figure>

In the next screen click the link to [register a new OAuth Application](https://github.com/settings/applications/new) in Github

<figure><img src="../../.gitbook/assets/image (11) (2).png" alt=""><figcaption></figcaption></figure>

In the Github page, complete the required fields and click **Register application**

| Field                      | Description                                                            |
| -------------------------- | ---------------------------------------------------------------------- |
| Application Name           | Your application name, for example you can use your organization name. |
| Homepage URL               | The url for your application or website,                               |
| Application Description    | Any description you choice                                             |
| Authorization Callback URL | Use https://localhost we will update this value later                  |

{% hint style="info" %}
You can complete the fields using the information suggested by terrakube in the VCS provider screen
{% endhint %}

<figure><img src="../../.gitbook/assets/image (9) (3) (2).png" alt=""><figcaption></figcaption></figure>

Next, generate a new client secret

<figure><img src="../../.gitbook/assets/image (8) (4).png" alt=""><figcaption></figcaption></figure>

Copy the  **Client Id**  and **Client Secret** from Github **** and **** go back to Terrakube to complete the **** required information. Then, click the **Connect and Continue** button

<figure><img src="../../.gitbook/assets/image (20).png" alt=""><figcaption></figcaption></figure>

In the next screen copy the **Callback URL,** we will need this value to update our github application

<figure><img src="../../.gitbook/assets/image (3) (1).png" alt=""><figcaption></figcaption></figure>

Go back to Github and update the callback URL, click the **Update application** button

<figure><img src="../../.gitbook/assets/image (6) (1) (4).png" alt=""><figcaption></figcaption></figure>

Once you updated the Callback Url in Github, return to Terrakube and click the **Connect to Github** button

<figure><img src="../../.gitbook/assets/image (3) (4).png" alt=""><figcaption></figcaption></figure>

You will see a new window, click the **Authorize** button to complete the connection

<figure><img src="../../.gitbook/assets/image (12) (1).png" alt=""><figcaption></figcaption></figure>

Finally if the connection was stablished successfully, you wil see a **Connected** message, you can close the window and return to Terrakube.

<figure><img src="../../.gitbook/assets/image (16).png" alt=""><figcaption></figcaption></figure>

If you refresh the VCS providers page in your organization, you should see the connection status with the date and the user that created the connection

<figure><img src="../../.gitbook/assets/image (10).png" alt=""><figcaption></figcaption></figure>

And now, you will be able to use the connection in your workspaces and modules:

<figure><img src="../../.gitbook/assets/image (18).png" alt=""><figcaption></figcaption></figure>
