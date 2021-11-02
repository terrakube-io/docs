# Step

This endpoint is used to see the output for a jobs step.&#x20;

### Entity fields:

| Path                       | Type   | Description                                                                              |
| -------------------------- | ------ | ---------------------------------------------------------------------------------------- |
| data.type                  | string | Should be "step"                                                                         |
| data.attributes.output     | string | URL with the output for the step                                                         |
| data.attributes.status     | string | Step status(pending or completed)                                                        |
| data.attributes.stepNumber | int    | A job can have several steps, this field is used to identity the order for the execution |

### Example:

```
{
  "data": {
    "type": "step",
    "attributes": {
      "output": "URL with the step output",
      "stepNumber": "Step Number Order",
      "status": "Terraform Provider"
    }
  }
}
```

### Supported Operations

{% swagger src="../../.gitbook/assets/v1_5.yml" path="/step/{stepId}" method="get" %}
[v1_5.yml](../../.gitbook/assets/v1_5.yml)
{% endswagger %}

{% hint style="info" %}
For a complete list of organization operation please visit the [OpenAPI specification](https://editor.swagger.io/?url=https://raw.githubusercontent.com/AzBuilder/terrakube-server/main/openapi-spec/v1\_5.yml)
{% endhint %}
