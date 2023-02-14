# Default Templates

Terrakube creates the following default templates:

## CLI-Driven Templates

Terrakube use job templates for all executions, Terrakube automatically create the following templates that are used when using the terraform remote state backend operations. This templates are created in all organizations.&#x20;

<figure><img src="../../../.gitbook/assets/image (60).png" alt=""><figcaption></figcaption></figure>

#### Terraform Plan/Apply Cli Template

Will be used when you execute [`terraform apply`](#user-content-fn-1)[^1] using the terraform cli

```yaml
flow:
- type: "terraformPlan"
  name: "Terraform Plan from Terraform CLI"
  step: 100
- type: "approval"
  name: "Approve Plan from Terraform CLI"
  step: 150
  team: "TERRAFORM_CLI"
- type: "terraformApply"
  name: "Terraform Apply from Terraform CLI"
  step: 200
```

#### Terraform Plan/Destroy Cli Templates

Will be used when you execute `terraform destroy`using the terraform cli

```yaml
flow:
- type: "terraformPlanDestroy"
  name: "Terraform Plan Destroy from Terraform CLI"
  step: 100
- type: "approval"
  name: "Approve Plan from Terraform CLI"
  step: 150
  team: "TERRAFORM_CLI"
- type: "terraformApply"
  name: "Terraform Apply from Terraform CLI"
  step: 200

```

{% hint style="warning" %}
This templates can be updated if need it but in order for the terraform remote state backed to work properly the step number and the template names should not be changed. So if you delete or modify this templates
{% endhint %}

[^1]: 
