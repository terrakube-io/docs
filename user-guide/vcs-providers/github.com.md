# Github

For using repositories from GitHub.com with Terrakube workspaces and modules you will need to follow these steps:

{% hint style="info" %}
**Manage VCS Providers** permission is required to perform this action, please check [team-management.md](../organizations/team-management.md "mention") for more info
{% endhint %}

Navigate to the desired organization **** and click the **Settings** button, then on the left menu select **VCS Providers**&#x20;

<figure><img src="../../.gitbook/assets/image (91).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
If you prefer, you can add a new VCS Provider directly from the Create workspace or Create Module screen.&#x20;
{% endhint %}

Click the **Github button**

<figure><img src="../../.gitbook/assets/image (132).png" alt=""><figcaption></figcaption></figure>

In the next screen click the link to [register a new OAuth Application](https://github.com/settings/applications/new) in Github

<figure><img src="../../.gitbook/assets/image (97).png" alt=""><figcaption></figcaption></figure>

In Github, complete the required fields and click **Register application**

| Field                      | Description                                                            |
| -------------------------- | ---------------------------------------------------------------------- |
| Application Name           | Your application name, for example you can use your organization name. |
| Homepage URL               | The url for your application or website,                               |
| Application Description    | Any description you choice                                             |
| Authorization Callback URL | Use https://localhost we will update this value later                  |

{% hint style="info" %}
You can complete the fields using the information suggested by terrakube in VCS providers screen
{% endhint %}

<figure><img src="../../.gitbook/assets/image (121).png" alt=""><figcaption></figcaption></figure>

Next, generate a new client secret

<figure><img src="../../.gitbook/assets/image (92).png" alt=""><figcaption></figcaption></figure>

Copy the  **Client Id**  and **Client Secret** from Github **** and **** go back to Terrakube to complete the **** required information. Next, click the **Connect and Continue** button

<figure><img src="../../.gitbook/assets/image (115).png" alt=""><figcaption></figcaption></figure>

In the next screen copy the **Callback URL,** we will need this value to update our github application

<figure><img src="../../.gitbook/assets/image (98).png" alt=""><figcaption></figcaption></figure>

Go back to Github and update the callback URL, click the **Update application**

<figure><img src="../../.gitbook/assets/image (137).png" alt=""><figcaption></figcaption></figure>

Once you updated the Callback Url in Github, return to Terrakube and click the **Connect to Github** button

<figure><img src="../../.gitbook/assets/image (143).png" alt=""><figcaption></figcaption></figure>

You will see a new window, click the **Authorize** button to complete the connection

<figure><img src="../../.gitbook/assets/image (102).png" alt=""><figcaption></figcaption></figure>

Finally if the connection was stablished successfully, you wil see **Connected** message, you can close the window and return to terrakube.

<figure><img src="../../.gitbook/assets/image (125).png" alt=""><figcaption></figcaption></figure>

If you refresh the VCS providers page in your organization, you should see the connection status with the date and the user that created the connection

<figure><img src="../../.gitbook/assets/image (109).png" alt=""><figcaption></figcaption></figure>

And now you will be able to use the connection in your workspaces and modules:

<figure><img src="../../.gitbook/assets/image (129).png" alt=""><figcaption></figcaption></figure>
