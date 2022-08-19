# Github

To connect Terrakube to GitHub and use private git repositories you will need to follow these steps:

### Step 1 - Register OAuth Application

Open GitHub and log in and use the following [link](https://github.com/settings/applications/new) to create a new application.

Complete the following fields:

* Application name
* Home page URL
* Authorization callback URL

{% hint style="info" %}
For application and authorization callback you can use http://localhost, we will update this value later.
{% endhint %}

Example:

![](<../.gitbook/assets/image (2) (1).png>)

After creating the application copy the **"Client Id"** and generate a new "**Client Secret"**.

### Step 2 - Create a new VCS Connection

Create a new VCS connection using the following values:

* clientId = GitHub Client Id
* clientSecret = GitHub Client Secret
* vcsType = GITHUB

{% hint style="warning" %}
Please refer to [VCS API](../api/methods/vcs.md) for more information
{% endhint %}

{% hint style="info" %}
You can also use Terrakube CLI or Terrakube UI to generate the VCS record.
{% endhint %}

Make sure to copy the VCS id from the response body.

### Step 3 - Update OAuth Application Callback

Update the application callback using the following structure:

```
https://{{TerrakubeApiURL}}/callback/v1/vcs/{{vcsId}}
```

### Step 4 - Authorize OAuth Application

Visit the following link

```
https://github.com/login/oauth/authorize?client_id={{GitHubClientId}}&allow_signup=false&scope=repo
```

Make sure to replace the values:

* GitHubClientId = Client Id from step 1

### Step 5 - Authorize Application

![](<../.gitbook/assets/image (1).png>)

If the setup was successful you should see this message in your browser.

```
Connected 
```
