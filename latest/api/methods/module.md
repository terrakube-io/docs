# Module

This endpoint is used to create, update, search or delete module information inside a Terrakube organization.

{% hint style="warning" %}
To use this endpoint you should have "manageModule" access at team level and a Terrakube organization
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
    "type": "module",
    "attributes": {
      "name": "Terrakube module",
      "description": "Module Description",
      "provider": "Terraform Provider",
      "source": "https://github.com/AzBuilder/terraform-sample-repository.git"
    },
        "relationships": {
            "vcs": {
                "data": {
                    "type": "vcs",
                    "id": "{{vcsId}}"
                }
            }
        }
  }
}
```

### Supported Operations

{% openapi src="../../.gitbook/assets/v2_6.yaml" path="/organization/{organizationId}/module/{moduleId}" method="get" %}
[v2_6.yaml](../../.gitbook/assets/v2_6.yaml)
{% endopenapi %}

{% openapi src="../../.gitbook/assets/v2_6.yaml" path="/organization/{organizationId}/module" method="post" %}
[v2_6.yaml](../../.gitbook/assets/v2_6.yaml)
{% endopenapi %}

{% openapi src="../../.gitbook/assets/v2_6.yaml" path="/organization/{organizationId}/module/{moduleId}" method="delete" %}
[v2_6.yaml](../../.gitbook/assets/v2_6.yaml)
{% endopenapi %}

{% openapi src="../../.gitbook/assets/v2_6.yaml" path="/organization/{organizationId}/module/{moduleId}" method="patch" %}
[v2_6.yaml](../../.gitbook/assets/v2_6.yaml)
{% endopenapi %}

{% hint style="info" %}
For a complete list of organization operation please visit the [OpenAPI specification](https://github.com/AzBuilder/terrakube-server/tree/main/openapi-spec)
{% endhint %}
