# Template

This endpoint is used to create, update, search or delete teamplates inside an organization.

### Entity fields:

| Path                       | Type   | Description                                   |
| -------------------------- | ------ | --------------------------------------------- |
| data.type                  | string | Should be "template"                          |
| data.attributes.tcl        | string | Terraform Configuration Language job template |
| data.attribute.name        | string | Template name                                 |
| data.attribute.description | string | Template description                          |
| data.attribute.version     | string | Template version                              |

{% hint style="info" %}
To better understand Terrakube Configuration Language job templates please refer to the following [GitHub repository.](https://github.com/AzBuilder/terrakube-extensions)
{% endhint %}

### Example:

```
{
  "data": {
    "type": "template",
    "attributes": {
      "name": "Some name",
      "description": "Some description",
      "version": "1.0.0",
      "tcl": "{{templateSample}}"
    }
  }
}
```

### Supported Operations

{% openapi src="../../.gitbook/assets/v2_6.yaml" path="/organization/{organizationId}/template" method="post" %}
[v2_6.yaml](../../.gitbook/assets/v2_6.yaml)
{% endopenapi %}

{% openapi src="../../.gitbook/assets/v2_6.yaml" path="/organization/{organizationId}/template/{templateId}" method="get" %}
[v2_6.yaml](../../.gitbook/assets/v2_6.yaml)
{% endopenapi %}

{% openapi src="../../.gitbook/assets/v2_6.yaml" path="/organization/{organizationId}/template/{templateId}" method="delete" %}
[v2_6.yaml](../../.gitbook/assets/v2_6.yaml)
{% endopenapi %}

{% openapi src="../../.gitbook/assets/v2_6.yaml" path="/organization/{organizationId}/template/{templateId}" method="patch" %}
[v2_6.yaml](../../.gitbook/assets/v2_6.yaml)
{% endopenapi %}

{% openapi src="../../.gitbook/assets/v2_6.yaml" path="/organization/{organizationId}/template" method="get" %}
[v2_6.yaml](../../.gitbook/assets/v2_6.yaml)
{% endopenapi %}

{% hint style="info" %}
For a complete list of organization operation please visit the [OpenAPI specification](https://github.com/AzBuilder/terrakube-server/blob/main/openapi-spec/v1_6.yml)
{% endhint %}
