# Azure DevOps

To connect Terrakube to your Azure DevOps and use private git repositories you will need to follow these steps:

### Step 1 - Register OAuth Application

Use the following [link](https://aex.dev.azure.com/app/register?mkt=en-US) to create a new OAuth Application inside Azure DevOps Services

Complete the following fields:

* Company name
* Application name
* Application website
* Authorization callback URL

{% hint style="info" %}
For authorization callback you can use http://localhost, we will update this value later.
{% endhint %}

Example:

![](../../.gitbook/assets/image%20%282%29.png)

Add the followings "Authorized scopes" and create the application:

* Code \(Read\)
* Code \(Status\)

Example: 

![](../../.gitbook/assets/image%20%281%29.png)

After creating the application copy the following values:

* App Id
* Client Secret

### Step 2 - Create a new VCS Connection

Create a new VCS connection using the following values:

* clientId = App ID 
* clientSecret = Client Secret 
* vcsType = AZURE\_DEVOPS

Example: 

```text
Http Method: POST
URL: {{TerrakubeApiURL}}/api/v1/organization/{{organizationId}}/vcs
Request Body:
{
  "data": {
    "type": "vcs",
    "attributes": {
      "name": "vcsAzDevOps",
      "description": "vcsDescription",
      "vcsType": "AZURE_DEVOPS",
      "clientId": "{{azDevOpsClientId}}",
      "clientSecret": "{{azDevOpsClientSecret}}"
    }
  }
}
Response Body Sample:
{
    "data": {
        "type": "vcs",
        "id": "{{vcsId}}",
        "attributes": {
            "clientId": "{{azDevOpsClientId}}",
            "description": "vcsDescription",
            "name": "vcsAzDevOps",
            "vcsType": "AZURE_DEVOPS"
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
You can also use Terrakube CLI or Terrakube UI
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
https://app.vssps.visualstudio.com/oauth2/authorize?client_id={{appId}}&redirect_uri={{appCallback}}&response_type=Assertion&scope=vso.code+vso.code_status
```

Make sure to replace the values:

* app\_id = Client Id from step 2
* redirect\_uri = Application callback from step 3

Step 5 - Authorize Application

![](../../.gitbook/assets/image%20%283%29.png)

If the setup was successful you should see this message in your browser.

```text
Connected 
```

