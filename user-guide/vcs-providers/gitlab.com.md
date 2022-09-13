# GitLab

For using repositories from Gitlab.com with Terrakube workspaces and modules you will need to follow these steps:

{% hint style="info" %}
**Manage VCS Providers** permission is required to perform this action, please check [team-management.md](../organizations/team-management.md "mention") for more info.
{% endhint %}



Navigate to the desired organization **** and click the **Settings** button, then on the left menu select **VCS Providers**&#x20;

<figure><img src="../../.gitbook/assets/image (14).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
If you prefer, you can add a new VCS Provider directly from the Create workspace or Create Module screen.&#x20;
{% endhint %}

Click the **Gitlab button**

<figure><img src="../../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

### Step 1 - Register OAuth Application

Open GitLab and log in and use the following [link](https://gitlab.com/-/profile/applications) and create a new application.

Complete the following fields:

* Application name
* Redirect URL
* Confidential
* Expire access token
* Scope (API)

{% hint style="info" %}
For application and authorization callback you can use http://localhost, we will update this value later.
{% endhint %}

Example:

![](<../../.gitbook/assets/image (10) (1) (1).png>)

After creating the application copy the **"Application Id"** and generate a new "**Secret"**.

### Step 2 - Create a new VCS Connection

Create a new VCS connection using the following values:

* clientId = Application Id
* clientSecret = Gitlab Application Secret
* vcsType = GITLAB

{% hint style="warning" %}
Please refer to [VCS API](../../api/methods/vcs.md) for more information
{% endhint %}

{% hint style="info" %}
You can also use Terrakube CLI or Terrakube UI to generate the VCS record.
{% endhint %}

Make sure to copy the VCS id from the response body.

### Step 3 - Update OAuth Application Callback

Update the application redirect URI using the following structure:

```
https://{{TerrakubeApiURL}}/callback/v1/vcs/{{vcsId}}
```

### Step 4 - Authorize OAuth Application

Visit the following link

```
https://gitlab.com/oauth/authorize?client_id={{applicationId}}&response_type=code&scope=api&&redirect_uri={{redirectURI}}
```

Make sure to replace the values:

* applicationId = Application Client Id from step 1
* redirectURI = Application callback from step 3

### Step 5 - Authorize Application

If the setup was successful you should see this message in your browser.

```
Connected 
```
