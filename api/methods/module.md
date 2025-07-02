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
      "source": "https://github.com/terrakube-io/terraform-sample-repository.git"
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

{% swagger src="https://raw.githubusercontent.com/terrakube-io/terrakube/main/openapi-spec/v1_3.yml" path="/organization/{organizationId}/module/{moduleId}" method="get" %}
[https://raw.githubusercontent.com/terrakube-io/terrakube/main/openapi-spec/v1_3.yml](https://raw.githubusercontent.com/terrakube-io/terrakube/main/openapi-spec/v1_3.yml)
{% endswagger %}

{% swagger src="https://raw.githubusercontent.com/terrakube-io/terrakube/main/openapi-spec/v1_3.yml" path="/organization/{organizationId}/module" method="post" %}
[https://raw.githubusercontent.com/terrakube-io/terrakube/main/openapi-spec/v1_3.yml](https://raw.githubusercontent.com/terrakube-io/terrakube/main/openapi-spec/v1_3.yml)
{% endswagger %}

{% swagger src="https://raw.githubusercontent.com/terrakube-io/terrakube/main/openapi-spec/v1_3.yml" path="/organization/{organizationId}/module/{moduleId}" method="delete" %}
[https://raw.githubusercontent.com/terrakube-io/terrakube/main/openapi-spec/v1_3.yml](https://raw.githubusercontent.com/terrakube-io/terrakube/main/openapi-spec/v1_3.yml)
{% endswagger %}

{% swagger src="https://raw.githubusercontent.com/terrakube-io/terrakube/main/openapi-spec/v1_3.yml" path="/organization/{organizationId}/module/{moduleId}" method="patch" %}
[https://raw.githubusercontent.com/terrakube-io/terrakube/main/openapi-spec/v1_3.yml](https://raw.githubusercontent.com/terrakube-io/terrakube/main/openapi-spec/v1_3.yml)
{% endswagger %}

{% hint style="info" %}
For a complete list of organization operation please visit the [OpenAPI specification](https://github.com/terrakube-io/terrakube-server/tree/main/openapi-spec)
{% endhint %}
