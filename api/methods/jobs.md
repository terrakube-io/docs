# Jobs

This endpoint is used to create, update, search or delete jobs for a particular Terrakube workspace.

{% hint style="warning" %}
To use this endpoint you need to create a Terrakube workspace.
{% endhint %}

### Entity fields:

| Path                                   | Type   | Description                                       |
| -------------------------------------- | ------ | ------------------------------------------------- |
| data.type                              | string | Should be "job"                                   |
| data.attributes.overrideBranch         | string | Branch that will be use to execute the job        |
| data.attributes.templateReference      | string | Terrakube template id to use when running the job |
| data.relationships.workspace.data.type | string | Should be "workspace"                             |
| data.relationships.workspace.data.Id   | string | Should be the VCS Workspace Id (OPTIONAL)         |

{% hint style="info" %}
To better understand Terrakube Configuration Language job templates please refer to the following [GitHub repository.](https://github.com/AzBuilder/terrakube-extensions)
{% endhint %}

### Example:

```
{
  "data": {
    "type": "job",
    "attributes": {
      "overrideBranch": "main"
      "templateReference": "XXXXXXXXX"
    },
    "relationships":{
        "workspace":{
            "data":{
                "type": "workspace",
                "id": "{{workspaceId}}"
            }
        }
    }
  }
}
```

### Supported Operations

{% openapi src="../../.gitbook/assets/v2_6.yaml" path="/organization/{organizationId}/job" method="post" %}
[v2_6.yaml](../../.gitbook/assets/v2_6.yaml)
{% endopenapi %}

{% openapi src="../../.gitbook/assets/v2_6.yaml" path="/organization/{organizationId}/job/{jobId}" method="get" %}
[v2_6.yaml](../../.gitbook/assets/v2_6.yaml)
{% endopenapi %}

{% openapi src="../../.gitbook/assets/v2_6.yaml" path="/organization/{organizationId}/job/{jobId}" method="patch" %}
[v2_6.yaml](../../.gitbook/assets/v2_6.yaml)
{% endopenapi %}

{% openapi src="../../.gitbook/assets/v2_6.yaml" path="/organization/{organizationId}/job/{jobId}" method="delete" %}
[v2_6.yaml](../../.gitbook/assets/v2_6.yaml)
{% endopenapi %}

{% hint style="info" %}
For a complete list of organization operation please visit the [OpenAPI specification](https://github.com/AzBuilder/terrakube-server/tree/main/openapi-spec)
{% endhint %}
