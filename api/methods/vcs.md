# VCS

This endpoint is used to create, update, search or delete vcs (version control system) information inside a Terrakube organization so you can connect to private git repositories to run modules and workspaces

{% hint style="warning" %}
To use this endpoint you should have "manageVcs" access at team level and a Terrakube Organization
{% endhint %}

### Entity fields:

| Path                         | Type   | Description                                               |
| ---------------------------- | ------ | --------------------------------------------------------- |
| data.type                    | string | Should be "vcs"                                           |
| data.attributes.name         | string | Unique vcs name for an Terrakube organization             |
| data.attributes.description  | string | Free text for description                                 |
| data.attributes.vcsType      | string | Supported values: GITHUB, GITLAB, BITBUCKET, AZURE_DEVOPS |
| data.attributes.clientID     | string | Client Application Id                                     |
| data.attributes.clientSecret | string | Client Application secret                                 |

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

{% swagger src="../../.gitbook/assets/v1_4.yml" path="/organization/{organizationId}/vcs/{vcsId}" method="get" %}
[v1_4.yml](../../.gitbook/assets/v1_4.yml)
{% endswagger %}

{% swagger src="../../.gitbook/assets/v1_4.yml" path="/organization/{organizationId}/vcs" method="post" %}
[v1_4.yml](../../.gitbook/assets/v1_4.yml)
{% endswagger %}

{% swagger src="../../.gitbook/assets/v1_4.yml" path="/organization/{organizationId}/vcs/{vcsId}" method="delete" %}
[v1_4.yml](../../.gitbook/assets/v1_4.yml)
{% endswagger %}

{% swagger src="../../.gitbook/assets/v1_4.yml" path="/organization/{organizationId}/vcs/{vcsId}" method="patch" %}
[v1_4.yml](../../.gitbook/assets/v1_4.yml)
{% endswagger %}

{% hint style="info" %}
For a complete list of organization operation please visit the [Open API specification.](https://editor.swagger.io/?url=https://raw.githubusercontent.com/AzBuilder/terrakube-server/main/openapi-spec/v1\_4.yml)
{% endhint %}
