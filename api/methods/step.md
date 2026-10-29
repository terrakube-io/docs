# Step

This endpoint is used to see the output for a job step.

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

{% openapi src="../../.gitbook/assets/terrakube-api.json" path="/organization/{organizationId}/job/{jobId}/step/{stepId}" method="get" %}
[terrakube-api.json](../../.gitbook/assets/terrakube-api.json)
{% endopenapi %}

{% hint style="info" %}
The complete OpenAPI specification is available as [terrakube-api.json](../../.gitbook/assets/terrakube-api.json), and in the UI under **Help & Support → API Docs**.
{% endhint %}
