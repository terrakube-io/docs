# Workspace

This endpoint is used to create, update, search or delete workspace information inside a Terrakube organization.&#x20;

{% hint style="warning" %}
To use this endpoint you should have "manageWorkspace" access at team level and a Terrakube organization
{% endhint %}

### Entity fields:

| Path                             | Type    | Description                                    |
| -------------------------------- | ------- | ---------------------------------------------- |
| data.type                        | string  | Should be "workspace"                          |
| data.attributes.name             | string  | Unique workspace for an Terrakube organization |
| data.attributes.branch           | boolean | Git branch to be used                          |
| data.attributes.source           | boolean | Git repository to be used                      |
| data.attributes.terraformVersion | boolean | Terraform version to run the workspace         |
| data.relationships.vcs.data.type | string  | Should be "vcs" (OPTIONAL)                     |
| data.relationships.vcs.data.type | string  | Should be the VCS Connection Id (OPTIONAL)     |

### Example:

```
{
    "data": {
        "type": "workspace",
        "attributes": {
            "branch": "main",
            "name": "Terrakube Workspace",
            "source": "https://github.com/AzBuilder/terraform-sample-repository.git",
            "terraformVersion": "0.15.0"
        },
        "relationships": {
            "vcs": {
                "data": {
                    "type": "vcs",
                    "id": "{{vcsIdGitHub}}"
                }
            }
        }
    }
}
```

### Supported Operations

{% swagger src="https://raw.githubusercontent.com/AzBuilder/azb-server/main/openapi-spec/v1_3.yml" path="/organization/{organizationId}/workspace/{workspaceId}" method="get" %}
[https://raw.githubusercontent.com/AzBuilder/azb-server/main/openapi-spec/v1_3.yml](https://raw.githubusercontent.com/AzBuilder/azb-server/main/openapi-spec/v1_3.yml)
{% endswagger %}

{% swagger src="https://raw.githubusercontent.com/AzBuilder/azb-server/main/openapi-spec/v1_3.yml" path="/organization/{organizationId}/workspace" method="post" %}
[https://raw.githubusercontent.com/AzBuilder/azb-server/main/openapi-spec/v1_3.yml](https://raw.githubusercontent.com/AzBuilder/azb-server/main/openapi-spec/v1_3.yml)
{% endswagger %}

{% swagger src="https://raw.githubusercontent.com/AzBuilder/azb-server/main/openapi-spec/v1_3.yml" path="/organization/{organizationId}/workspace/{workspaceId}" method="delete" %}
[https://raw.githubusercontent.com/AzBuilder/azb-server/main/openapi-spec/v1_3.yml](https://raw.githubusercontent.com/AzBuilder/azb-server/main/openapi-spec/v1_3.yml)
{% endswagger %}

{% swagger src="https://raw.githubusercontent.com/AzBuilder/azb-server/main/openapi-spec/v1_3.yml" path="/organization/{organizationId}/workspace/{workspaceId}" method="patch" %}
[https://raw.githubusercontent.com/AzBuilder/azb-server/main/openapi-spec/v1_3.yml](https://raw.githubusercontent.com/AzBuilder/azb-server/main/openapi-spec/v1_3.yml)
{% endswagger %}

{% hint style="info" %}
For a complete list of organization operation please visit the [OpenAPI specification](https://github.com/AzBuilder/terrakube-server/blob/main/openapi-spec/v1\_6.yml)
{% endhint %}

