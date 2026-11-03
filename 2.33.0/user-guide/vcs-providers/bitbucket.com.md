---
description: Work in progress
---

# Bitbucket

For using repositories from Bitbucket.com with Terrakube workspaces and modules you will need to follow these steps:

{% hint style="info" %}
**Manage VCS Providers** permission is required to perform this action, please check [team-management.md](../organizations/team-management.md "mention") for more info.
{% endhint %}

Navigate to the desired organization and click the **Settings** button, then on the left menu select **VCS Providers**&#x20;

<figure><img src="../../.gitbook/assets/image (385).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
If you prefer, you can add a new VCS Provider directly from the [Create workspace](../workspaces/creating-workspaces.md) or Create Module screen.
{% endhint %}

Click the **Bitbucket** button and then click the **Bitbucket Cloud** option

<figure><img src="../../.gitbook/assets/image (74).png" alt=""><figcaption></figcaption></figure>

Open [Bitbucket Cloud](https://bitbucket.org) and log in as whichever account you want Terrakube to use and click the **settings** button in your workspace

<figure><img src="../../.gitbook/assets/image (142).png" alt=""><figcaption></figcaption></figure>

Click the **OAuth consumers** menu and then click the **Add consumer** button

<figure><img src="../../.gitbook/assets/image (146).png" alt=""><figcaption></figcaption></figure>

Complete the required fields and click **Save**  button

<table><thead><tr><th width="219">Field</th><th>Description</th></tr></thead><tbody><tr><td>Name</td><td>Your application name, for example you can use your organization name.</td></tr><tr><td>Description</td><td>Any description of your choice</td></tr><tr><td>Redirect URI</td><td>Copy the Redirect URI from the Terrakube UI</td></tr><tr><td>This is a private consumer (checkbox)</td><td>Checked</td></tr><tr><td>Permissions (checkboxes)</td><td><p>The following should be checked: </p><p><strong>Account</strong>: Write </p><p><strong>Repositories</strong>: Admin </p><p><strong>Pull requests</strong>: Write </p><p><strong>Webhooks</strong>: Read and write</p></td></tr></tbody></table>

{% hint style="info" %}
You can complete the fields using the information suggested by terrakube in the VCS provider screen
{% endhint %}

<figure><img src="../../.gitbook/assets/image (300).png" alt=""><figcaption></figcaption></figure>

In the next screen, copy the **Key** and **Secret**

<figure><img src="../../.gitbook/assets/image (413).png" alt=""><figcaption></figcaption></figure>

Go back to Terrakube to enter the information you copied from the previous step. Then, click the **Connect and Continue** button.

<figure><img src="../../.gitbook/assets/image (324).png" alt=""><figcaption></figcaption></figure>

You will see a Bitbucket window, click the **Grant Access** button to complete the connection

<figure><img src="../../.gitbook/assets/image (400).png" alt=""><figcaption></figcaption></figure>

Finally, if the connection was established successfully, you will be redirected to the VCS provider’s page in your organization. You should see the connection status with the date and the user that created the connection.

<figure><img src="../../.gitbook/assets/image (158).png" alt=""><figcaption></figcaption></figure>

And now, you will be able to use the connection in your workspaces and modules:

<figure><img src="../../.gitbook/assets/image (411).png" alt=""><figcaption></figcaption></figure>
