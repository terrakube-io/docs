---
description: Work in progress
---

# Bitbucket

To connect Terrakube to Bitbucket and use private git repositories you will need to follow these steps:

### Step 1 - Register OAuth Application

Open [Bitbucket Cloud](https://bitbucket.org) and log in as whichever account you want Terrakube to use.

Navigate to Bitbucket's "Add OAuth Consumer" page.

{% hint style="info" %}
[https://bitbucket.org/](https://bitbucket.org)\{{WORKSPACE NAME\}}/workspace/settings/oauth-consumers/new
{% endhint %}

You can also reach it through Bitbucket's menus:

* Click your profile picture and choose the workspace you want to access.
* In the workspace navigation, click "Settings".
* In the settings navigation, click "OAuth consumers,"
* On the OAuth settings page and click the "Add consumer" button.

Complete the following fields:

* Application name
* Application URL
* Authorization callback URL

{% hint style="info" %}
For application and authorization callback you can use http://localhost, we will update this value later.
{% endhint %}

Example:

![](<../.gitbook/assets/image (2) (1).png>)

Check "This is a private consumer" and add the following permission:

* Repository (Read)

Example:

![](<../.gitbook/assets/image (6) (1) (1).png>)

After creating the application copy the following values:

* Key
* Secret

### Step 2 - Create a new VCS Connection

Create a new VCS connection using the following values:

* clientId = Key
* clientSecret = Client Secret
* vcsType = BITBUCKET

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
https://bitbucket.org/site/oauth2/authorize?client_id={{keyId}}&response_type=code&response_type=code&scope=repository
```

Make sure to replace the values:

* keyID = Application Key from step 1

### Step 5 - Authorize Application

![](<../.gitbook/assets/image (4) (1) (1).png>)

If the setup was successful you should see this message in your browser.

```
Connected 
```
