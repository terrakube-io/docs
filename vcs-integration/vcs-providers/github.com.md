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

![](../../.gitbook/assets/image%20%282%29.png)

After creating the application copy the **"Client Id"** and generate a new "**Client Secret"**.

### Step 2 - Create a new VCS Connection

Create a new VCS connection using the following values:

* clientId = GitHub Client Id 
* clientSecret = GitHub Client Secret 
* vcsType = GITHUB

Example: 

```text
Http Method: POST
URL: {{TerrakubeApiURL}}/api/v1/organization/{{organizationId}}/vcs
Request Body:
{
  "data": {
    "type": "vcs",
    "attributes": {
      "name": "vcsGitHub",
      "description": "vcsDescription",
      "vcsType": "GITHUB",
      "clientId": "{{githubClientId}}",
      "clientSecret": "{{githubClientSecret}}"
    }
  }
}
Response Body Sample:
{
    "data": {
        "type": "vcs",
        "id": "{{vcsId}}",
        "attributes": {
            "clientId": "{{githubClientId}}",
            "description": "vcsDescription",
            "name": "vcsGithub",
            "vcsType": "GITHUB"
        },
        "relationships": {
            "organization": {
                "data": {
                    "type": "organization",
                    "id": "{{organizationId}}"
                }
            }
        }
    }
}
```

{% hint style="info" %}
You can also use Terrakube CLI or Terrakube UI to generate the VCS record.
{% endhint %}

Make sure to copy the VCS id from the response body.

### Step 3 - Update OAuth Application Callback

Update the application callback using the following structure:

```text
https://{{TerrakubeApiURL}}/callback/v1/vcs/{{vcsId}}
```

### Step 4 - Authorize OAuth Application

Visit the following link

```text
https://github.com/login/oauth/authorize?client_id={{GitHubClientId}}&allow_signup=false&scope=repo
```

Make sure to replace the values:

* GitHubClientId = Client Id from step 1

### Step 5 - Authorize Application

![](../../.gitbook/assets/image%20%281%29.png)

If the setup was successful you should see this message in your browser.

```text
Connected 
```

