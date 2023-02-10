# CLI-driven Workflow (Templates)

Terrakube use job templates for all executions, Terrakube automatically create the following templates that are used when using the terraform remote state backend operations. This templates are created in all organizations.

<figure><img src="../../.gitbook/assets/image (60).png" alt=""><figcaption></figcaption></figure>

#### Terraform Apply Template

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

#### Terraform Destroy Templates

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
**This templates can be updated if need it but in order for the terraform remote state backed to work properly the step number and the template names should not be changed**
{% endhint %}
