# Jobs

This endpoint is used to create, update, search or delete jobs for a particular Terrakube workspace.

{% hint style="warning" %}
To use this endpoint you need to create a Terrakube workspace.
{% endhint %}

### Entity fields:

| Path                                   | Type   | Description                                       |
| -------------------------------------- | ------ | ------------------------------------------------- |
| data.type                              | string | Should be "job"                                   |
| data.attributes.tcl                    | string | Terraform Configuration Language job template     |
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
      "tcl": "Base64 TCL job template"
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

{% swagger src="https://raw.githubusercontent.com/AzBuilder/azb-server/main/openapi-spec/v1_3.yml" path="/workspace/{workspaceId}/job/{jobId}" method="get" %}
[https://raw.githubusercontent.com/AzBuilder/azb-server/main/openapi-spec/v1_3.yml](https://raw.githubusercontent.com/AzBuilder/azb-server/main/openapi-spec/v1_3.yml)
{% endswagger %}

{% swagger src="https://raw.githubusercontent.com/AzBuilder/azb-server/main/openapi-spec/v1_3.yml" path="/workspace/{workspaceId}/job" method="post" %}
[https://raw.githubusercontent.com/AzBuilder/azb-server/main/openapi-spec/v1_3.yml](https://raw.githubusercontent.com/AzBuilder/azb-server/main/openapi-spec/v1_3.yml)
{% endswagger %}

{% swagger src="https://raw.githubusercontent.com/AzBuilder/azb-server/main/openapi-spec/v1_3.yml" path="/workspace/{workspaceId}/job/{jobId}" method="delete" %}
[https://raw.githubusercontent.com/AzBuilder/azb-server/main/openapi-spec/v1_3.yml](https://raw.githubusercontent.com/AzBuilder/azb-server/main/openapi-spec/v1_3.yml)
{% endswagger %}

{% swagger src="https://raw.githubusercontent.com/AzBuilder/azb-server/main/openapi-spec/v1_3.yml" path="/workspace/{workspaceId}/job/{jobId}" method="patch" %}
[https://raw.githubusercontent.com/AzBuilder/azb-server/main/openapi-spec/v1_3.yml](https://raw.githubusercontent.com/AzBuilder/azb-server/main/openapi-spec/v1_3.yml)
{% endswagger %}

{% hint style="info" %}
For a complete list of organization operation please visit the [OpenAPI specification](https://github.com/AzBuilder/terrakube-server/tree/main/openapi-spec)
{% endhint %}
