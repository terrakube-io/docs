# Drift Detection

Terrakube can be used to detect any infrastructure drift, this can be done using Terrakube extensions(open policy and slack), templates and schedules, below you can find an example of how this can be achieved.&#x20;

### Open Policy Definition

The firts step will be to create a small rego policy and add this to your Terrakube extensions repository inside the "policy" folder with the name **"plan\_information.rego"**, this is a very simple policy that will count the number of changes for a terraform plan.

```
package terrakube.plan.information

import input as tfplan
  
created := create_count {
    resources := [resource | resource:= tfplan.resource_changes[_]; resource.change.actions[_] == "create"]
    create_count := count(resources)
}

deleted := delete_count {
    resources := [resource | resource:= tfplan.resource_changes[_]; resource.change.actions[_] == "delete"]
    delete_count := count(resources)
}

updated := updated_count {
    resources := [resource | resource:= tfplan.resource_changes[_]; resource.change.actions[_] == "update"]
    updated_count := count(resources)
}

no_change := no_change_count {
    resources := [resource | resource:= tfplan.resource_changes[_]; resource.change.actions[_] == "no-op"]
    no_change_count := count(resources)
```

The output of this policy will look like this:

```json
{
    "created": 2,
    "deleted": 4,
    "no_change": 2,
    "updated": 5
}
```

The policy should look like this in your extension repository

<figure><img src="../.gitbook/assets/image (309).png" alt=""><figcaption></figcaption></figure>

{% hint style="warning" %}
Terrakube extensions can be stored inside a GIT repository that you can configure when staring the platform. This is an example repository that you can fork or customize to create your custom extensions based on your own requirements [https://github.com/AzBuilder/terrakube-extensions](https://github.com/AzBuilder/terrakube-extensions)
{% endhint %}

### Template Definition

The firts step will be to create a Terrakube template that validate if there is a change in our infrastructure.

<pre class="language-yaml"><code class="lang-yaml"><strong>flow:
</strong>  - type: "terraformPlan"
    step: 100
    name: "Running Terraform Plan with Drift Detection and Slack Notification"
<strong>    commands:
</strong>      - runtime: "GROOVY"
        priority: 100
        after: true
        script: |
          import Opa

          new Opa().loadTool(
            "$workingDirectory",
            "$bashToolsDirectory",
            "0.45.0")
          "Opa Download Completed..."
      - runtime: "BASH"
        priority: 200
        after: true
        script: |
          cd $workingDirectory;
          terraform show -json terraformLibrary.tfPlan > tfplan.json;
          echo "Validating terraform plan information";
          opa exec --decision terrakube/plan/information --bundle .terrakube/toolsRepository/policy/ tfplan.json | jq '.result[0].result' > drift_detection.json;
          cat drift_detection.json;
      - runtime: "GROOVY"
        priority: 300
        after: true
        script: |
          import SlackApp
          import groovy.json.JsonSlurper
          import groovy.json.JsonOutput
          
          File drift_detection = new File("${workingDirectory}/drift_detection.json")
          String drift_detection_content = drift_detection.text

          println drift_detection_content

          def jsonSlurper = new JsonSlurper()
          def body = jsonSlurper.parseText(drift_detection_content)
          def changes =  body.created + body.updated + body.deleted

          if (changes > 0) {
            new SlackApp().sendMessageWithoutAttachment(
              "#general", 
              "Hello team, Terrakube has deteted an infrastructure drift, please review the following workspace $workspaceId inside organization $organizationId", 
              "$SLACK_TOKEN", 
              terrakubeOutput);
          } else {
            new SlackApp().sendMessageWithoutAttachment(
              "#general", 
              "Hello team, Terrakube did not detect any infrastructure drift for workspace $workspaceId inside organization $organizationId", 
              "$SLACK_TOKEN", 
              terrakubeOutput);
          }

          "Drift Detection Completed..."
</code></pre>

In a high level this template will do the following:

* Run a terraform plan

```
flow:
  - type: "terraformPlan"
    step: 100
    name: "Running Terraform Plan with Drift Detection and Slack Notification"
```

* Once the terraform plan is completed, the template will import the Open Policy Extension to our job workspace

```
    commands:
      - runtime: "GROOVY"
        priority: 100
        after: true
        script: |
          import Opa

          new Opa().loadTool(
            "$workingDirectory",
            "$bashToolsDirectory",
            "0.45.0")
          "Opa Download Completed..."
```

* The next step will be to export the terraform plan in JSON format and execute the rego policy to validate the number of changes in our terraform plan and create a file **"drift\_detection.json"** with the result

```
      - runtime: "BASH"
        priority: 200
        after: true
        script: |
          cd $workingDirectory;
          terraform show -json terraformLibrary.tfPlan > tfplan.json;
          echo "Validating terraform plan information";
          opa exec --decision terrakube/plan/information --bundle .terrakube/toolsRepository/policy/ tfplan.json | jq '.result[0].result' > drift_detection.json;
          cat drift_detection.json;
      
```

* Now you can use the **"drift\_detection.json"** file using a simple groovy script to review the number of changes in the terraform plan.

```
      - runtime: "GROOVY"
        priority: 300
        after: true
        script: |
          import SlackApp
          import groovy.json.JsonSlurper
          import groovy.json.JsonOutput
          
          File drift_detection = new File("${workingDirectory}/drift_detection.json")
          String drift_detection_content = drift_detection.text

          println drift_detection_content

          def jsonSlurper = new JsonSlurper()
          def body = jsonSlurper.parseText(drift_detection_content)
          def changes =  body.created + body.updated + body.deleted

```

* Once you have the number of changes we can add a simple validation to send a Slack Message using the Terrakube extension to notify you teams if an infrastructure drift was detected

```
          if (changes > 0) {
            new SlackApp().sendMessageWithoutAttachment(
              "#general", 
              "Hello team, Terrakube has deteted an infrastructure drift, please review the following workspace $workspaceId inside organization $organizationId", 
              "$SLACK_TOKEN", 
              terrakubeOutput);
          } else {
            new SlackApp().sendMessageWithoutAttachment(
              "#general", 
              "Hello team, Terrakube did not detect any infrastructure drift for workspace $workspaceId inside organization $organizationId", 
              "$SLACK_TOKEN", 
              terrakubeOutput);
          }
```

{% hint style="info" %}
```
SLACK_TOKEN is an environment variable that you can define at workspace level.
You can also setup this using the global variables in your organization
```
{% endhint %}

### Template Setup

Now that you have define the template to detect the infrastructure drift you can add the template in your Terrakube organization using the name "Drift Detection"

The template will look like this:

<figure><img src="../.gitbook/assets/image (388).png" alt=""><figcaption></figcaption></figure>

Now you can use this template in any workspace inside your organization

<figure><img src="../.gitbook/assets/image (410).png" alt=""><figcaption></figcaption></figure>

### Workspace Setup

To test the template we can use the following example for Azure and we execute a Terraform Apply inside Terrakube. This will create a app service plan with tier **Basic** and size **B1**

```
terraform {
  required_providers {
    azurerm = {
      source  = "hashicorp/azurerm"
      version = "=3.25.0"
    }
  }
}

provider "azurerm" {
  features {}
}

resource "azurerm_resource_group" "example" {
  name     = "api-rg-pro"
  location = "East Us 2"
}

resource "azurerm_app_service_plan" "example" {
  name                = "api-appserviceplan-pro"
  location            = azurerm_resource_group.example.location
  resource_group_name = azurerm_resource_group.example.name

  sku {
    tier = "Basic"
    size = "B1"
  }
}
```

Once our resources are created in Azure we can run the **Drift Detection** template.

<figure><img src="../.gitbook/assets/image (227).png" alt=""><figcaption></figcaption></figure>

The template will send a message to a Slack channel with the following:

* If there is no infracstructure change you should receive the following message in your slack channel.

<figure><img src="../.gitbook/assets/image (113).png" alt=""><figcaption></figcaption></figure>

* If for some reason the resource in Azure is changed (scale up) and our  state in Terrakube does not match with Azure you will see the following message.

<figure><img src="../.gitbook/assets/image (131).png" alt=""><figcaption></figcaption></figure>

### Schedule Drift Detection

Now that you have tested the Drift Detection template, you can use with the **Workspace Schedule** feature to run this template for example every day at 5:30 AM for the workspace.

You will have to go the the **"Schedule"** option inside your workspace.

<figure><img src="../.gitbook/assets/image (209).png" alt=""><figcaption></figcaption></figure>

Now you can select the **Drift Detection** template to run at 5:30 AM every day.

<figure><img src="../.gitbook/assets/image (213).png" alt=""><figcaption></figcaption></figure>

Now you should receive the notification in your Slack channel every day at 5:30 am&#x20;

Implementing Drift detection in Terrakube is easy, you just need to make use of extension and write a small script, this is just an example of easy is to extend Terrakube functionality using extensions, you can even create more complex templates quickly, for example you could create a webhook or send emails using the sendgrid extension.

### Handling errors
One could also be tempted to return a slack message when a drift template ended with an error. By default, Terrakube
template execution will stop when encountering an error. This behavior can be modified by setting `ignoreError` parameter.

```
flow:
  - type: "terraformPlan"
    step: 100
    name: "Running Terraform Plan with Drift Detection and Slack Notification"
    ignoreError: true
    commands:
    ... rest of your drift template
```
