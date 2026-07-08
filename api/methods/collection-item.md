# Collection Item

This endpoint is used to manage collection items for a collection inside the organization.

### Entity fields:

| Path                        | Type    | Description                                    |
| --------------------------- | ------- | ---------------------------------------------- |
| data.type                   | string  | Should be "item"                               |
| data.attributes.key         | string  | Variable key name                              |
| data.attributes.value       | string  | Variable value                                 |
| data.attributes.description | string  | Item description                               |
| data.attributes.category    | string  | Variable category: "ENV" or "TERRAFORM"        |
| data.attributes.sensitive   | boolean | To hide the value when the output is sensitive |
| data.attributes.hcl         | boolean | Whether the value is HCL formatted             |

{% hint style="warning" %}
When `sensitive` is set to `true`, the `value` field will not be returned in GET responses. Sensitive values cannot be retrieved after creation.
{% endhint %}

### Example

```json
POST /api/v1/organization/${ORGANIZATION_ID}/collection/${COLLECTION_ID}/item

{
    "data": {
        "type": "item",
        "attributes": {
            "key": "random_key",
            "value": "random_value",
            "description": "random_description",
            "category": "ENV",
            "sensitive": true,
            "hcl": false
        }
    }
}
```

### Supported Operations

{% openapi src="../../.gitbook/assets/v2_7.yaml" path="/organization/{organizationId}/collection/{collectionId}/item" method="get" %}
[v2_7.yaml](../../.gitbook/assets/v2_7.yaml)
{% endopenapi %}

{% openapi src="../../.gitbook/assets/v2_7.yaml" path="/organization/{organizationId}/collection/{collectionId}/item" method="post" %}
[v2_7.yaml](../../.gitbook/assets/v2_7.yaml)
{% endopenapi %}

{% openapi src="../../.gitbook/assets/v2_7.yaml" path="/organization/{organizationId}/collection/{collectionId}/item/{itemId}" method="get" %}
[v2_7.yaml](../../.gitbook/assets/v2_7.yaml)
{% endopenapi %}

{% openapi src="../../.gitbook/assets/v2_7.yaml" path="/organization/{organizationId}/collection/{collectionId}/item/{itemId}" method="delete" %}
[v2_7.yaml](../../.gitbook/assets/v2_7.yaml)
{% endopenapi %}

{% openapi src="../../.gitbook/assets/v2_7.yaml" path="/organization/{organizationId}/collection/{collectionId}/item/{itemId}" method="patch" %}
[v2_7.yaml](../../.gitbook/assets/v2_7.yaml)
{% endopenapi %}
