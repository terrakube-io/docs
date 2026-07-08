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

{% openapi src="../../.gitbook/assets/v2_6.yaml" path="/organization" method="get" %}
[v2_6.yaml](../../.gitbook/assets/v2_6.yaml)
{% endopenapi %}

{% openapi src="../../.gitbook/assets/v2_1.yml" path="/organization" method="post" %}
[v2_1.yml](../../.gitbook/assets/v2_1.yml)
{% endopenapi %}

{% openapi src="../../.gitbook/assets/v2_1.yml" path="/organization/{organizationId}" method="delete" %}
[v2_1.yml](../../.gitbook/assets/v2_1.yml)
{% endopenapi %}

{% openapi src="../../.gitbook/assets/v2_1.yml" path="/organization/{organizationId}" method="patch" %}
[v2_1.yml](../../.gitbook/assets/v2_1.yml)
{% endopenapi %}

{% hint style="info" %}
For a complete list of organization operation please visit the [OpenAPI specification](https://github.com/AzBuilder/terrakube-server/tree/main/openapi-spec)
{% endhint %}
