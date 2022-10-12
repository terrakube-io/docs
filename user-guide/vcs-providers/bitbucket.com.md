---
description: Work in progress
---

# Bitbucket

For using repositories from Bitbucket.com with Terrakube workspaces and modules you will need to follow these steps:

{% hint style="info" %}
**Manage VCS Providers** permission is required to perform this action, please check [team-management.md](../organizations/team-management.md "mention") for more info.
{% endhint %}

Navigate to the desired organization **** and click the **Settings** button, then on the left menu select **VCS Providers**&#x20;

<figure><img src="../../.gitbook/assets/image (14) (2).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
If you prefer, you can add a new VCS Provider directly from the Create workspace or Create Module screen.
{% endhint %}

Click the **Bitbucket** button

<figure><img src="../../.gitbook/assets/image (21).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (5) (3).png" alt=""><figcaption></figcaption></figure>

Open [Bitbucket Cloud](https://bitbucket.org) and log in as whichever account you want Terrakube to use and click the **settings** button in your workspace

<figure><img src="../../.gitbook/assets/image (58).png" alt=""><figcaption></figcaption></figure>

Click the **OAuth consumers** menu and then click the **Add consumer** button

<figure><img src="../../.gitbook/assets/image (3) (3).png" alt=""><figcaption></figcaption></figure>

Complete the required fields and click **Save**  button

| Field                                 | Description                                                                                                                                                                                                              |
| ------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Name                                  | Your application name, for example you can use your organization name.                                                                                                                                                   |
| Description                           | Any description of your choice                                                                                                                                                                                           |
| Redirect URI                          | Use https://localhost we will update this value later                                                                                                                                                                    |
| This is a private consumer (checkbox) | Checked                                                                                                                                                                                                                  |
| Permissions (checkboxes)              | <p>The following should be checked: </p><p><strong>Account</strong>: Write </p><p><strong>Repositories</strong>: Admin </p><p><strong>Pull requests</strong>: Write </p><p><strong>Webhooks</strong>: Read and write</p> |

{% hint style="info" %}
You can complete the fields using the information suggested by terrakube in the VCS provider screen
{% endhint %}

<figure><img src="../../.gitbook/assets/image (8).png" alt=""><figcaption></figcaption></figure>

In the next screen, copy the **Key** and **Secret**

<figure><img src="../../.gitbook/assets/image (13).png" alt=""><figcaption></figcaption></figure>

Go back to Terrakube to enter the information you copied from the previous step. Then, click the **Connect and Continue** button.

<figure><img src="../../.gitbook/assets/image (2) (4).png" alt=""><figcaption></figcaption></figure>

In the next screen copy the **Callback URL,** we will need this value to update our Bitbucket application

<figure><img src="../../.gitbook/assets/image (2) (3).png" alt=""><figcaption></figcaption></figure>

Go back to Bitbucket and update the Callback URL, click the **Save** button

<figure><img src="../../.gitbook/assets/image (59).png" alt=""><figcaption></figcaption></figure>

Once you updated the Callback Url in Bitbucket, return to Terrakube and click the **Connect to Bitbucket** button

<figure><img src="../../.gitbook/assets/image (4) (3).png" alt=""><figcaption></figcaption></figure>

You will see a new window, click the **Grant Access** button to complete the connection

<figure><img src="../../.gitbook/assets/image (15).png" alt=""><figcaption></figcaption></figure>

Finally if the connection was stablished successfully, you wil see a **Connected** message, you can close the window and return to Terrakube.

<figure><img src="../../.gitbook/assets/image (6) (1) (5).png" alt=""><figcaption></figcaption></figure>

If you refresh the VCS providers page in your organization, you should see the connection status with the date and the user that created the connection

<figure><img src="../../.gitbook/assets/image (40).png" alt=""><figcaption></figcaption></figure>

And now, you will be able to use the connection in your workspaces and modules:

<figure><img src="../../.gitbook/assets/image (9).png" alt=""><figcaption></figcaption></figure>
