# Persistent Context

Persistent Context is helpfull when you need to save information from the job execution, for example it can be used to save the infracost or save the thread id when using the Slack extension. We can also use it to save any JSON information generated inside Terrakube Jobs.

#### Using Persistent Contexts with the Terrakube API

In order to save the information the terrakube API exposes the following endpoint

```
POST {{terrakubeApi}}/context/v1/{{jobId}}
{
  "slackThreadId": "12345667",
  "infracost": {}
}
```

{% hint style="warning" %}
Job context can only be updated when the status of the Job is running.
{% endhint %}

To get the context you can use the Terrakube API

```
GET {{terrakubeApi}}/context/v1/{{jobId}}
```

#### Using Persistent Contexts in Templates

The persistent context can be used using the [Context](https://github.com/AzBuilder/terrakube-extensions/tree/main/groovy/Context) extension from the Terrakube extension repository. It supports saving a JSON file or saving a new property inside the context JSON.

```
import Context
        
new Context("$terrakubeApi", "$terrakubeToken", "$jobId", "$workingDirectory").saveFile("infracost", "infracost.json")
new Context("$terrakubeApi", "$terrakubeToken", "$jobId", "$workingDirectory").saveProperty("slackId", "1234567890")
```

This is an example of a Terrakube template using the persistent job context to save the infracost information.

```
flow:
- type: "terraformPlan"
  step: 100
  commands:
    - runtime: "GROOVY"
      priority: 100
      after: true
      script: |
        import Infracost

        String credentials = "version: \"0.1\"\n" +
                "api_key: $INFRACOST_KEY \n" +
                "pricing_api_endpoint: https://pricing.api.infracost.io"

        new Infracost().loadTool(
           "$workingDirectory",
           "$bashToolsDirectory", 
           "0.10.12",
           credentials)
        "Infracost Download Completed..."
    - runtime: "BASH"
      priority: 200
      after: true
      script: |
        terraform show -json terraformLibrary.tfPlan > plan.json 
        INFRACOST_ENABLE_DASHBOARD=true infracost breakdown --path plan.json --format json --out-file infracost.json
    - runtime: "GROOVY"
      priority: 300
      after: true
      script: |
        import Context
        
        new Context("$terrakubeApi", "$terrakubeToken", "$jobId", "$workingDirectory").saveFile("infracost", "infracost.json")

        "Save context completed..."
```

You can use persistent context to customize the Job UI, see [UI templates ](ui-templates.md)for more details.

