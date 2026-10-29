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
To better understand Terrakube Configuration Language job templates please refer to the following [GitHub repository.](https://github.com/terrakube-io/terrakube-extensions)
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

{% openapi src="../../.gitbook/assets/terrakube-api.json" path="/organization/{organizationId}/template" method="post" %}
[terrakube-api.json](../../.gitbook/assets/terrakube-api.json)
{% endopenapi %}

{% openapi src="../../.gitbook/assets/terrakube-api.json" path="/organization/{organizationId}/template/{templateId}" method="get" %}
[terrakube-api.json](../../.gitbook/assets/terrakube-api.json)
{% endopenapi %}

{% openapi src="../../.gitbook/assets/terrakube-api.json" path="/organization/{organizationId}/template/{templateId}" method="delete" %}
[terrakube-api.json](../../.gitbook/assets/terrakube-api.json)
{% endopenapi %}

{% openapi src="../../.gitbook/assets/terrakube-api.json" path="/organization/{organizationId}/template/{templateId}" method="patch" %}
[terrakube-api.json](../../.gitbook/assets/terrakube-api.json)
{% endopenapi %}

{% openapi src="../../.gitbook/assets/terrakube-api.json" path="/organization/{organizationId}/template" method="get" %}
[terrakube-api.json](../../.gitbook/assets/terrakube-api.json)
{% endopenapi %}

{% hint style="info" %}
The complete OpenAPI specification is available as [terrakube-api.json](../../.gitbook/assets/terrakube-api.json), and in the UI under **Help & Support → API Docs**.
{% endhint %}
