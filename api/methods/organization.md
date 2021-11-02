# Organization

This endpoint is used to create, update, search or delete organizations information inside Terrakube.

{% hint style="info" %}
You need to be part of the administrator Active Directory Group to work with this endpoint
{% endhint %}

{% hint style="warning" %}
The default group is handled using the Spring Boot property&#x20;

```
org.azbuilder.owner=AZB_ADMIN
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

{% swagger src="https://raw.githubusercontent.com/AzBuilder/terrakube-server/main/openapi-spec/v1_4.yml" path="/organization/{organizationId}/vcs/{vcsId}" method="get" %}
[https://raw.githubusercontent.com/AzBuilder/terrakube-server/main/openapi-spec/v1_4.yml](https://raw.githubusercontent.com/AzBuilder/terrakube-server/main/openapi-spec/v1_4.yml)
{% endswagger %}

{% swagger src="../../.gitbook/assets/v1_4.yml" path="/organization/{organizationId}/vcs" method="post" %}
[v1_4.yml](../../.gitbook/assets/v1_4.yml)
{% endswagger %}

{% swagger src="../../.gitbook/assets/v1_4.yml" path="/organization/{organizationId}/vcs/{vcsId}" method="delete" %}
[v1_4.yml](../../.gitbook/assets/v1_4.yml)
{% endswagger %}

{% swagger src="../../.gitbook/assets/v1_4.yml" path="/organization/{organizationId}/vcs/{vcsId}" method="patch" %}
[v1_4.yml](../../.gitbook/assets/v1_4.yml)
{% endswagger %}

{% hint style="info" %}
For a complete list of organization operation please visit the [OpenAPI specification](https://editor.swagger.io/?url=https://raw.githubusercontent.com/AzBuilder/terrakube-server/main/openapi-spec/v1\_5.yml)[ ](https://editor.swagger.io/?url=https://raw.githubusercontent.com/AzBuilder/terrakube-server/main/openapi-spec/v1\_4.yml)
{% endhint %}
