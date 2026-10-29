# Vcs

This endpoint is used to create, update, search or delete vcs (version control system) information inside a Terrakube organization so you can connect to private git repositories to run modules and workspaces

{% hint style="warning" %}
To use this endpoint you should have "manageVcs" access at team level and a Terrakube Organization
{% endhint %}

### Entity fields:

| Path                         | Type   | Description                                                |
| ---------------------------- | ------ | ---------------------------------------------------------- |
| data.type                    | string | Should be "vcs"                                            |
| data.attributes.name         | string | Unique vcs name for an Terrakube organization              |
| data.attributes.description  | string | Free text for description                                  |
| data.attributes.vcsType      | string | Supported values: GITHUB, GITLAB, BITBUCKET, AZURE\_DEVOPS |
| data.attributes.clientID     | string | Client Application Id                                      |
| data.attributes.clientSecret | string | Client Application secret                                  |

### Example:

```
{
  "data": {
    "type": "vcs",
    "attributes": {
      "name": "GitHubConnection",
      "description": "Private repositories inside GitHub",
      "vcsType": "GITHUB",
      "clientId": "{{githubClientId}}",
      "clientSecret": "{{githubClientSecret}}"
    }
  }
}
```

### Supported Operations

{% openapi src="../../.gitbook/assets/terrakube-api.json" path="/organization/{organizationId}/vcs/{vcsId}" method="get" %}
[terrakube-api.json](../../.gitbook/assets/terrakube-api.json)
{% endopenapi %}

{% openapi src="../../.gitbook/assets/terrakube-api.json" path="/organization/{organizationId}/vcs" method="post" %}
[terrakube-api.json](../../.gitbook/assets/terrakube-api.json)
{% endopenapi %}

{% openapi src="../../.gitbook/assets/terrakube-api.json" path="/organization/{organizationId}/vcs/{vcsId}" method="delete" %}
[terrakube-api.json](../../.gitbook/assets/terrakube-api.json)
{% endopenapi %}

{% openapi src="../../.gitbook/assets/terrakube-api.json" path="/organization/{organizationId}/vcs/{vcsId}" method="patch" %}
[terrakube-api.json](../../.gitbook/assets/terrakube-api.json)
{% endopenapi %}

{% hint style="info" %}
The complete OpenAPI specification is available as [terrakube-api.json](../../.gitbook/assets/terrakube-api.json), and in the UI under **Help & Support → API Docs**.
{% endhint %}
