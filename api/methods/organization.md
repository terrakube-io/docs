# Organization

This endpoint is used to create, update, search or delete organizations information inside Terrakube.

{% hint style="info" %}
You need to be part of the administrator Active Directory Group to work with this endpoint
{% endhint %}

{% hint style="warning" %}
The default group is handled using the Spring Boot property

```
org.azbuilder.owner=TERRAKUBE_ADMIN
```
{% endhint %}

### Entity fields:

| Path                        | Type   | Description              |
| --------------------------- | ------ | ------------------------ |
| data.type                   | string | Should be "organization" |
| data.attributes.name        | string | Unique name in Terrakube |
| data.attributes.description | string | Free description         |

### Example:

```
{
  "data": {
    "type": "organization",
    "attributes": {
      "name": "terrakube",
      "description": "Terrakube Organization"
    }
  }
}
```

### Supported Operations

{% openapi src="../../.gitbook/assets/terrakube-api.json" path="/organization" method="get" %}
[terrakube-api.json](../../.gitbook/assets/terrakube-api.json)
{% endopenapi %}

{% openapi src="../../.gitbook/assets/terrakube-api.json" path="/organization" method="post" %}
[terrakube-api.json](../../.gitbook/assets/terrakube-api.json)
{% endopenapi %}

{% openapi src="../../.gitbook/assets/terrakube-api.json" path="/organization/{organizationId}" method="delete" %}
[terrakube-api.json](../../.gitbook/assets/terrakube-api.json)
{% endopenapi %}

{% openapi src="../../.gitbook/assets/terrakube-api.json" path="/organization/{organizationId}" method="patch" %}
[terrakube-api.json](../../.gitbook/assets/terrakube-api.json)
{% endopenapi %}

{% hint style="info" %}
The complete OpenAPI specification is available as [terrakube-api.json](../../.gitbook/assets/terrakube-api.json), and in the UI under **Help & Support → API Docs**.
{% endhint %}
