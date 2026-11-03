# Import Templates

Templates can become big over time while adding steps to execute additional business logic.

Example before template import:

```yaml
flow:
- type: "terraformPlan"
  step: 100
  commands:
    - runtime: "GROOVY"
      priority: 100
      before: true
      script: |
        import TerraTag
        new TerraTag().loadTool(
          "$workingDirectory",
          "$bashToolsDirectory",
          "0.1.30")
        "Terratag download completed"
    - runtime: "BASH"
      priority: 200
      before: true
      script: |
        cd $workingDirectory
        terratag -tags="{\"environment_id\": \"development\"}"
- type: "terraformApply"
  step: 300
```

Terrakube supports having the template logic in an external git repository like the following:

{% hint style="info" %}
This feature is only available from Terrakube 2.11.0
{% endhint %}

```yaml
flow:
  - type: "terraformPlan"
    step: 100
    importComands:
      repository: "https://github.com/terrakube-io/terrakube-extensions"
      folder: "templates/terratag"
      branch: "import-template"
  - type: "terraformApply"
    step: 200
```

Using import templates become simply and we can reuse the logic accross several Terrakube organizations

The sample template can be found [here](https://github.com/terrakube-io/terrakube-extensions/blob/main/templates/terratag/commands.yaml).

{% hint style="info" %}
The import commands are using a public repository, we will add support for private repositories in the future.
{% endhint %}
