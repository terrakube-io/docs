# Github Enterprise

To use Terrakube’s VCS features with a self-hosted GitHub Enterprise, follow these instructions.  For GitHub.com, [see the separate instructions](github.com.md).

{% hint style="info" %}
**Manage VCS Providers** permission is required to perform this action, please check [team-management.md](../organizations/team-management.md "mention") for more info.
{% endhint %}

* Got to the organization settings you want to configure.  Select the **VCS Providers** on the left menu and click the **Add VCS provider** button on the top right corner..

<figure><img src="../../.gitbook/assets/image (359).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
If you prefer, you can add a new VCS Provider directly from the [Create workspace](../workspaces/creating-workspaces.md) or Create Module screen.&#x20;
{% endhint %}

* Click the **Github button** and then click the **Github Enterprise option.**

<figure><img src="../../.gitbook/assets/image (357).png" alt=""><figcaption></figcaption></figure>

* On the next screen, enter the following information about your Github Enterprise instance:

<table><thead><tr><th width="240">Field </th><th>Value</th></tr></thead><tbody><tr><td>HTTP URL</td><td><code>https://&#x3C;GITHUB INSTANCE HOSTNAME></code></td></tr><tr><td>API URL</td><td><code>https://&#x3C;GITHUB INSTANCE HOSTNAME>/api/v3</code></td></tr></tbody></table>

<figure><img src="../../.gitbook/assets/image (360).png" alt=""><figcaption></figcaption></figure>

* Use a different browser tab to access your GitHub Enterprise instance and sign in with the account that you want Terrakube to use. You should use a service user account for your organization, but you can also use a personal account.

{% hint style="info" %}
Note: The account that connects Terrakube with your GitHub Enterprise instance needs to have admin rights to any shared Terraform configuration repositories. This is because creating webhooks requires admin permissions.
{% endhint %}

* Navigate to GitHub's Register a New OAuth Application page. This page is located at `https://<GITHUB INSTANCE HOSTNAME>/settings/applications/new`
* In the Github Enterprise page, complete the required fields and click **Register application**

<table><thead><tr><th width="354">Field</th><th>Description</th></tr></thead><tbody><tr><td>Application Name</td><td>Your application name, for example you can use your organization name.</td></tr><tr><td>Homepage URL</td><td>The url for your application or website,</td></tr><tr><td>Application Description</td><td>Any description you choice</td></tr><tr><td>Authorization Callback URL</td><td>Copy the callback url from the Terrakube UI</td></tr></tbody></table>

{% hint style="info" %}
You can complete the fields using the information suggested by terrakube in the VCS provider screen
{% endhint %}



<figure><img src="../../.gitbook/assets/image (361).png" alt=""><figcaption></figcaption></figure>

Next, generate a new client secret

<figure><img src="../../.gitbook/assets/image (362).png" alt=""><figcaption></figcaption></figure>

Copy the  **Client Id**  and **Client Secret** from Github Enterprise and go back to Terrakube to complete the required information. Then, click the **Connect and Continue** button

<figure><img src="../../.gitbook/assets/image (363).png" alt=""><figcaption></figcaption></figure>

You will be redirected to Github Enterprise. Click the **Authorize** button to complete the connection.

<figure><img src="../../.gitbook/assets/image (364).png" alt=""><figcaption></figcaption></figure>

When the connection is successful, you will go back to the VCS provider’s page in your organization. You will see the connection status, the date, and the user who made the connection.&#x20;

<figure><img src="../../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

You can now use the connection for your workspaces and modules.

<figure><img src="../../.gitbook/assets/image (1) (1).png" alt=""><figcaption></figcaption></figure>
