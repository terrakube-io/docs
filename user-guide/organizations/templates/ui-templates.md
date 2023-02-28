# UI Templates

Terrakube allows you to customize the UI for each step inside your templates using standard HTML. So you can render any kind of content extracted from your Job execution in the Terrakube UI.

For example you can  present the costs using Infracost in a friendly way:

<figure><img src="../../../.gitbook/assets/image (16) (4).png" alt=""><figcaption></figcaption></figure>

Or present a table with the OPA policies&#x20;

<figure><img src="../../../.gitbook/assets/image (2) (3).png" alt=""><figcaption></figcaption></figure>

In order to use UI templates you will need to save the HTML for each template step using the [Persistent Context](persistent-context.md). Terrakube expects the ui templates in the following format.

```
terrakubeUI :{
   "100" : "<span>Some Content</span>"
}
```

In the above example the 100 property in the JSON refers to the step number inside your template. In order to save this value from the template you can use the Context extension. For example:

```
flow:
  - type: "terraformPlan"
    step: 100
    name: "Plan"
    commands:
      - runtime: "GROOVY"
        priority: 300
        after: true
        script: |
          import Context
          
          def uiTemplate = '{"100":"<span>Simple Text</span>"}'
          new Context("$terrakubeApi", "$terrakubeToken", "$jobId", "$workingDirectory").saveProperty("terrakubeUI", uiTemplate)

          "Save context completed..."
  - type: "terraformApply"
    step: 200
    name: "Apply"
```



