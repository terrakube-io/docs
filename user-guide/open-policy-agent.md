# Open Policy Agent

Terrakube can be integrated with Open Policy Agent using the Terrakube extension to validate the Terraform plan, below you can find an example of how this can be achieved. The example is based on this [document](https://www.openpolicyagent.org/docs/latest/terraform/#goals).&#x20;

### Terraform Policy

The firts step will be to create some terraform policies inside your Terrakube extensions repository folder, inside the policy folder as you can see in the image below.

<figure><img src="../.gitbook/assets/image (39).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (39).png" alt=""><figcaption></figcaption></figure>

{% hint style="warning" %}
Terrakube extensions can be stored inside a GIT repository that you can configure when staring the platform. This is an example repository that you can fork or customiza to create your custom extensions [https://github.com/AzBuilder/terrakube-extensions](https://github.com/AzBuilder/terrakube-extensions)
{% endhint %}

The following policy computes a score for a Terraform that combines

* The number of deletions of each resource type
* The number of creations of each resource type
* The number of modifications of each resource type

The policy authorizes the plan when the score for the plan is below a threshold and there are no changes made to any IAM resources. (For simplicity, the threshold in this tutorial is the same for everyone, but in practice you would vary the threshold depending on the user.)

#### **policy/terraform.rego**

```
package terraform.analysis

import input as tfplan

########################
# Parameters for Policy
########################

# acceptable score for automated authorization
blast_radius := 30

# weights assigned for each operation on each resource-type
weights := {
    "aws_autoscaling_group": {"delete": 100, "create": 10, "modify": 1},
    "aws_instance": {"delete": 10, "create": 1, "modify": 1}
}

# Consider exactly these resource types in calculations
resource_types := {"aws_autoscaling_group", "aws_instance", "aws_iam", "aws_launch_configuration"}

#########
# Policy
#########

# Authorization holds if score for the plan is acceptable and no changes are made to IAM
default authz := false
authz {
    score < blast_radius
    not touches_iam
}

# Compute the score for a Terraform plan as the weighted sum of deletions, creations, modifications
score := s {
    all := [ x |
            some resource_type
            crud := weights[resource_type];
            del := crud["delete"] * num_deletes[resource_type];
            new := crud["create"] * num_creates[resource_type];
            mod := crud["modify"] * num_modifies[resource_type];
            x := del + new + mod
    ]
    s := sum(all)
}

# Whether there is any change to IAM
touches_iam {
    all := resources["aws_iam"]
    count(all) > 0
}

####################
# Terraform Library
####################

# list of all resources of a given type
resources[resource_type] := all {
    some resource_type
    resource_types[resource_type]
    all := [name |
        name:= tfplan.resource_changes[_]
        name.type == resource_type
    ]
}

# number of creations of resources of a given type
num_creates[resource_type] := num {
    some resource_type
    resource_types[resource_type]
    all := resources[resource_type]
    creates := [res |  res:= all[_]; res.change.actions[_] == "create"]
    num := count(creates)
}


# number of deletions of resources of a given type
num_deletes[resource_type] := num {
    some resource_type
    resource_types[resource_type]
    all := resources[resource_type]
    deletions := [res |  res:= all[_]; res.change.actions[_] == "delete"]
    num := count(deletions)
}

# number of modifications to resources of a given type
num_modifies[resource_type] := num {
    some resource_type
    resource_types[resource_type]
    all := resources[resource_type]
    modifies := [res |  res:= all[_]; res.change.actions[_] == "update"]
    num := count(modifies)
}
```

### Template Definition

Once we have defined one or more policies inside your Terrakube extension repository, yoa can write a custom template that make use of the Open Policy Agent using the Terrakube Configuration Language as the following example:

<pre><code>flow:
  - type: "terraformPlan"
    step: 100
    name: "Terraform Plan with OPA Check"
    commands:
      - runtime: "GROOVY"
        priority: 100
        after: true
        script: |
          import Opa

          new Opa().loadTool(
            "$workingDirectory",
            "$bashToolsDirectory",
            "0.44.0")
          "Opa Download Completed..."
<strong>      - runtime: "BASH"
</strong>        priority: 200
        after: true
        script: |
          cd $workingDirectory;
          terraform show -json terraformLibrary.tfPlan > tfplan.json;
          echo "Checkint Open Policy Agent Policy";
          opaCheck=$(opa exec --decision terraform/analysis/authz --bundle .terrakube/toolsRepository/policy/ tfplan.json | jq '.result[0].result');

          if [ "$opaCheck" == "true" ]; then
            echo "Policy is valid"
            exit 0
          else
            echo "Policy is invalid"
            exit 1
          fi</code></pre>

In a high level this template will do the following:

* Run the terraform plan for the Terrakube workspace

```
...
- type: "terraformPlan"
    step: 100
    name: "Terraform Plan with OPA Check"
...
```

* After the terraform plan is completed successfully it will import the Open Policy Agent inside our job using the Opa Groovy extension.

```
...
    commands:
      - runtime: "GROOVY"
        priority: 100
        after: true
        script: |
          import Opa

          new Opa().loadTool(
            "$workingDirectory",
            "$bashToolsDirectory",
            "0.44.0")
          "Opa Download Completed..."
...
```

* Once the Open Policy Agent has been imported successfully inside our Terrakube Job, you can make use of it to validate different terraform policies defined inside the Terrakube extensions repository that gets imported inside our workspace job using a simple BASH script. Based on the result of the policy you can difine if the job flow can continue or not.

<pre><code>...
<strong>      - runtime: "BASH"
</strong>        priority: 200
        after: true
        script: |
          cd $workingDirectory;
          
          terraform show -json terraformLibrary.tfPlan > tfplan.json;
          echo "Checkint Open Policy Agent Policy";
          opaCheck=$(opa exec --decision terraform/analysis/authz --bundle .terrakube/toolsRepository/policy/ tfplan.json | jq '.result[0].result');

          if [ "$opaCheck" == "true" ]; then
            echo "Policy is valid"
            exit 0
          else
            echo "Policy is invalid"
            exit 1
          fi
..</code></pre>

You can add this template inside your organization settings using a custom template and following these steps.

* Create a new template

<figure><img src="../.gitbook/assets/image (3) (3) (1).png" alt=""><figcaption></figcaption></figure>

* Select a blank template.

<figure><img src="../.gitbook/assets/image (2) (1) (2).png" alt=""><figcaption></figcaption></figure>

* Add the template body.

<figure><img src="../.gitbook/assets/image (27).png" alt=""><figcaption></figcaption></figure>

* Save the template

<figure><img src="../.gitbook/assets/image (1) (1) (3).png" alt=""><figcaption></figcaption></figure>

### Terrakube Workspace Code

Now that your terraform policy is completed and you have defined the Terrakube template that make us of the Open Policy agent, you can star testing it with some sample workspaces.

#### Example 1:

Create a Terrakube Workspace with the terraform file that includes an auto-scaling group and a server on AWS. (You will need to define the variables AWS\_ACCESS\_KEY\_ID and AWS\_SECRET\_ACCESS\_KEY to access your AWS account to run the terraform plan)

```
provider "aws" {
    region = "us-west-1"
}
resource "aws_instance" "web" {
  instance_type = "t2.micro"
  ami = "ami-09b4b74c"
}
resource "aws_autoscaling_group" "my_asg" {
  availability_zones        = ["us-west-1a"]
  name                      = "my_asg"
  max_size                  = 5
  min_size                  = 1
  health_check_grace_period = 300
  health_check_type         = "ELB"
  desired_capacity          = 4
  force_delete              = true
  launch_configuration      = "my_web_config"
}
resource "aws_launch_configuration" "my_web_config" {
    name = "my_web_config"
    image_id = "ami-09b4b74c"
    instance_type = "t2.micro"
}
```

When you run the Terrakube job for this workspace you can see that the terraform policy is valid for our workspace.

<figure><img src="../.gitbook/assets/image (49).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (52).png" alt=""><figcaption></figcaption></figure>

#### Example 2:

Create a Terrakube  plan that creates enough resources to exceed the blast-radius permitted by the terraform policy.  (You will need to define the variables AWS\_ACCESS\_KEY\_ID and AWS\_SECRET\_ACCESS\_KEY to access your AWS account to run the terraform plan)

```
provider "aws" {
    region = "us-west-1"
}
resource "aws_instance" "web" {
  instance_type = "t2.micro"
  ami = "ami-09b4b74c"
}
resource "aws_autoscaling_group" "my_asg" {
  availability_zones        = ["us-west-1a"]
  name                      = "my_asg"
  max_size                  = 5
  min_size                  = 1
  health_check_grace_period = 300
  health_check_type         = "ELB"
  desired_capacity          = 4
  force_delete              = true
  launch_configuration      = "my_web_config"
}
resource "aws_launch_configuration" "my_web_config" {
    name = "my_web_config"
    image_id = "ami-09b4b74c"
    instance_type = "t2.micro"
}
resource "aws_autoscaling_group" "my_asg2" {
  availability_zones        = ["us-west-2a"]
  name                      = "my_asg2"
  max_size                  = 6
  min_size                  = 1
  health_check_grace_period = 300
  health_check_type         = "ELB"
  desired_capacity          = 4
  force_delete              = true
  launch_configuration      = "my_web_config"
}
resource "aws_autoscaling_group" "my_asg3" {
  availability_zones        = ["us-west-2b"]
  name                      = "my_asg3"
  max_size                  = 7
  min_size                  = 1
  health_check_grace_period = 300
  health_check_type         = "ELB"
  desired_capacity          = 4
  force_delete              = true
  launch_configuration      = "my_web_config"
}
```

When you run this Terrakube job for this particular workspace you will see that the terraform policy is invalid and the job flow will failed.

<figure><img src="../.gitbook/assets/image (53).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (54).png" alt=""><figcaption></figcaption></figure>

This is how you can make use of Terrakube extension and Open Policy Agent to validate your terraform resources inside your organization before you deployed it to the cloud.&#x20;

Terrakube templates are very flexible and can be used to create more complex scenarios, in a real world escenario if the policy is valid you can proceed to deploy your infrastructure like this template where you can add an additional step to run the terraform apply.

```
flow:
  - type: "terraformPlan"
    step: 100
    name: "Terraform Plan with OPA Check"
    commands:
      - runtime: "GROOVY"
        priority: 100
        after: true
        script: |
          import Opa

          new Opa().loadTool(
            "$workingDirectory",
            "$bashToolsDirectory",
            "0.44.0")
          "Opa Download Completed..."
      - runtime: "BASH"
        priority: 200
        after: true
        script: |
          cd $workingDirectory;
          terraform show -json terraformLibrary.tfPlan > tfplan.json;
          echo "Checkint Open Policy Agent Policy";
          opaCheck=$(opa exec --decision terraform/analysis/authz --bundle .terrakube/toolsRepository/policy/ tfplan.json | jq '.result[0].result');

          if [ "$opaCheck" == "true" ]; then
            echo "Policy is valid"
            exit 0
          else
            echo "Policy is invalid"
            exit 1
          fi
  - type: "terraformApply"
    step: 300
 
```
