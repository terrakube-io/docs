# Templates

Templates allows you to customize the job workflow inside each workspace. You can define multiple templates inside each organization. Templates are written in yaml and you can specify each step to be executed once you use this template in the workspace. Lets see an example for a basic template:

#### Example 1:

```
flow:
  - name: "Plan"
    type: "terraformPlan"
    step: 100
  - name: "Apply"
    type: "terraformApply"
    step: 200
```

#### Example 2:

```
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

#### Example 3:

```
flow:
  - type: "terraformDestroy"
    step: 100
  - type: "disableWorkspace"
    step: 200
```

If you notice inside the **flow** section you can define any step required, for this case only 2 steps are defined. Using the **type** property you can specify the kind of step to be performed, in the above case we are using some default types like **terraformPlan** and **terraformApply.** However the power of terrakube is that you can extend your workflow to execute additional tasks using bash or groovy code. Lets see a more advanced template:

```
flow:
  - name: "Plan"
    type: "terraformPlan"
    step: 100
    commands:
      - runtime: "GROOVY"
        priority: 100
        before: true
        script: |
          @Grapes([
            @Grab('commons-io:commons-io:2.8.0'),
            @Grab('org.apache.commons:commons-compress:1.21'),
          ])

          import org.apache.commons.io.FileUtils
          
          class TerraTagDownloader {
            def downloadTerraTag(workingDirectory, version, os, arch) {
              String terraTagFile = "terratag_${version}_${os}_${arch}.tar.gz"
              String terraTagURL = "https://github.com/env0/terratag/releases/download/v${version}/${terraTagFile}"
              println "Downloading $terraTagURL"
              FileUtils.copyURLToFile(new URL(terraTagURL), new File("${workingDirectory}/${terraTagFile}"))
            }
          } 
          new TerraTagDownloader().downloadTerraTag("$workingDirectory", "0.1.29", "darwin", "amd64")
          "TerraTag Download Compledted..."
      - runtime: "BASH"
        priority: 200
        before: true
        script: |
          cd $workingDirectory;
          tar -xvf terratag_0.1.29_darwin_amd64.tar.gz;
          chmod +x terratag;
          ./terratag -tags="{\"environment_id\": \"development\"}"
  - name: "Apply"
    type: "terraformApply"
    step: 300
  - name: "Destroy"
    type: "terraformDestroy"
    step: 400
```

The previous example is using [Terratag](https://www.terratag.io/) to add the environment\_id to all the resources in the workspace. To acomplish that, the template defines some additional commands during the **Terraform Plan,** in this case a groovy class is utilized to download the Terratag binary, and a bash script is used to execute the terratag cli to append the required tags.

Using templates you can define all the business logic you need to execute in your workspace, basically you can integrate terrakube with any external tool. For example you can use [Infracost](https://www.infracost.io/) to calculate the cost of the resources to be created in your workspace or verify some policies using [Open Policy Agent](https://www.openpolicyagent.org/).

{% hint style="warning" %}
Terrakube extensions can be stored inside a GIT repository that you can configure when staring the platform. This is an example repository that you can fork or customiza to create your custom extensions [https://github.com/AzBuilder/terrakube-extensions](https://github.com/AzBuilder/terrakube-extensions)
{% endhint %}

There are some tools that are very common, and you don't want repeat the same code everytime or maybe other engineer already created the code and the logic to integrate with a specific tool. For these cases, inside the template is possible to reuse some logic using [Terrakube extensions](https://github.com/AzBuilder/terrakube-extensions). Basically if someone else developed an integration with an external third party service, you don't need to implement it again, you can use the extension. Let's see how the template looks like using the [Terratag extension](https://github.com/AzBuilder/terrakube-extensions/tree/main/groovy/TerraTag):

```
flow:
  - name: "Plan"
    type: "terraformPlan"
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
```

If you notice the above script is more compact as is reusing some logic. If you want to simplify and reuse some templates in your organization, please check [Import Templates](import-templates.md).

Using Templates you can even customize the UI for each job, see [UI templates](ui-templates.md) for more details.

{% hint style="info" %}
To see the complete list for terrakube extensions see the [github repo](https://github.com/AzBuilder/terrakube-extensions). This repo is always growing so its possible the tool you need is already there. In this repo you can also see more templates examples for things like approvals, variable injection and so on.
{% endhint %}

### Persistent Context

If you need to store information generated on the steps defined in your template, you can use [persistent context](persistent-context.md).

### Creating a Template

{% hint style="info" %}
**Manage VCS Templates** permission is required to perform this action, please check [team-management.md](../team-management.md "mention") for more info
{% endhint %}

Once you are in the desired organization, click the **Settings** button, then in the left menu select the **Templates** option and click the **Add Template** button

<figure><img src="../../../.gitbook/assets/image (397).png" alt=""><figcaption></figcaption></figure>

You will see a list of some predefined templates than you can utilize as a quick start or if you prefer you can click the **Blank Template** to start from scratch

<figure><img src="../../../.gitbook/assets/image (134).png" alt=""><figcaption></figcaption></figure>

In the next screen you can define your template and when you are ready click the **Continue** button.

<figure><img src="../../../.gitbook/assets/image (148).png" alt=""><figcaption></figcaption></figure>

Finally, you can assign a name and description to your template and click the **Create Template** button.

<figure><img src="../../../.gitbook/assets/image (124).png" alt=""><figcaption></figcaption></figure>

Now you template is ready and you can start using it in any workspace within your organization.

<figure><img src="../../../.gitbook/assets/image (412).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
By default Terrakube don't create any template, so you have to define the templates in your organization based on your requirements.
{% endhint %}

### Editing a Template

Click the **Edit** button next to the Template you would like to edit

<figure><img src="../../../.gitbook/assets/image (135).png" alt=""><figcaption></figcaption></figure>

Edit your template definition, description or name as desired and when you are ready click the **Save Template** button

<figure><img src="../../../.gitbook/assets/image (393).png" alt=""><figcaption></figcaption></figure>

### Deleting a Template

Click the **Delete** button next to the Template you want to delete and then click **Yes** to confirm the deletion

<figure><img src="../../../.gitbook/assets/image (117).png" alt=""><figcaption></figcaption></figure>
