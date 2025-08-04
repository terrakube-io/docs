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

{% openapi-operation spec="terrakube-api" path="/organization/{organizationId}/vcs/{vcsId}" method="get" %}
[OpenAPI terrakube-api](https://gitbook-x-prod-openapi.4401d86825a13bf607936cc3a9f3897a.r2.cloudflarestorage.com/raw/cb9b759d0a5961decbc26bf547567df170cc7d69216b1d84e1fdc034f053cda2.json?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=dce48141f43c0191a2ad043a6888781c%2F20250804%2Fauto%2Fs3%2Faws4_request&X-Amz-Date=20250804T151109Z&X-Amz-Expires=172800&X-Amz-Signature=b8e5244069f0315750515e4bb8cf20ae1917938f41a4c0e65a72ac76f51b0751&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)
{% endopenapi-operation %}

{% openapi src="../../.gitbook/assets/v1_4.yml" path="/organization/{organizationId}/vcs" method="post" %}
[v1_4.yml](../../.gitbook/assets/v1_4.yml)
{% endopenapi %}

{% openapi src="../../.gitbook/assets/v1_4.yml" path="/organization/{organizationId}/vcs/{vcsId}" method="delete" %}
[v1_4.yml](../../.gitbook/assets/v1_4.yml)
{% endopenapi %}

{% openapi src="../../.gitbook/assets/v1_4.yml" path="/organization/{organizationId}/vcs/{vcsId}" method="patch" %}
[v1_4.yml](../../.gitbook/assets/v1_4.yml)
{% endopenapi %}

{% hint style="info" %}
For a complete list of organization operation please visit the [OpenAPI specification](https://github.com/AzBuilder/terrakube-server/tree/main/openapi-spec)
{% endhint %}
