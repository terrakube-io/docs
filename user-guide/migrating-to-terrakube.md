# Migrating to Terrakube

When migrating to Terrakube you can use the following approaches:

* [**Migrating with the Terraform CLI**](migrating-to-terrakube.md#migrating-with-the-terraform-cli): Ideal for users who prefer to migrate workspaces individually, leveraging the familiar Terraform CLI.
* [**Migrating with the Workspaces Importer**](migrating-to-terrakube.md#migrating-with-the-workspaces-importer): Provides a guided, wizard-like experience within Terrakube, enabling you to seamlessly import workspaces from platforms such as Terraform Cloud or Terraform Enterprise.

### Migrating with the Terraform CLI

1. Begin by downloading the current state from Terraform Cloud / Enterprise with the following command:

```
terraform state pull > tf.state
```

2. Proceed to modify your Terraform configuration to introduce Terrakube as the replacement cloud backend. It's crucial to include the `hostname` parameter:

{% tabs %}
{% tab title="remote backend" %}
```
terraform {
  backend "remote" {
    hostname = "8080-terrakube-io-terrakube-q8aleg88vlc.ws-us92.gitpod.io"
    organization = "migrate-org"

    workspaces {
      name = "migrate-state"
    }
  }
}
```
{% endtab %}

{% tab title="cloud block" %}
```
terraform {
  cloud {
    hostname = "8080-terrakube-io-terrakube-q8aleg88vlc.ws-us92.gitpod.io"
    organization = "migrate-org"
    workspaces {
      name = "migrate-state"
    }
  }
}
```
{% endtab %}
{% endtabs %}

3. And run the following commands

```
terraform login 8080-terrakube-io-terrakube-q8aleg88vlc.ws-us92.gitpod.io
terraform init 
terraform state push tf.state

```

Once the migration process is completed you should see the terraform state in your storage backend (azure, aws, gcp or minio) depending of your configuration

### **Migrating with the Workspaces Importer**

To begin migrating your workspaces using the Workspace Importer within Terrakube, follow these steps:

1. Navigate to the Workspaces List within your organization and click on "Import Workspaces".

<figure><img src="../.gitbook/assets/image (446).png" alt=""><figcaption></figcaption></figure>

2. **Select Your Platform**: Choose the platform where your workspaces are currently hosted. Terrakube currently supports both Terraform Cloud and Terraform Enterprise.

<figure><img src="../.gitbook/assets/image (447).png" alt=""><figcaption></figcaption></figure>

3. **Choose Your Workflow**: Decide on the workflow type you wish to use for the import. If you need to import from multiple workflows, you will need to execute the importer for each one separately.

<figure><img src="../.gitbook/assets/image (448).png" alt=""><figcaption></figcaption></figure>

4. **Configure VCS Workflow (If Applicable)**: For a Version control workflow, select your VCS provider connection. For guidance on connecting your VCS provider, refer to the VCS provider section.

<figure><img src="../.gitbook/assets/image (449).png" alt=""><figcaption></figcaption></figure>



5. **Enter API Keys**: Provide your Terraform Cloud or Terraform Enterprise API keys. For Terraform Enterprise, you will also need to supply your host URL. For more information, consult the Terraform Cloud documentation.

<figure><img src="../.gitbook/assets/image (450).png" alt=""><figcaption></figcaption></figure>

6. **Select Workspaces for Import**: A list of available workspaces will be displayed. Select the ones you wish to import and then click the "Import Workspaces" button.

<figure><img src="../.gitbook/assets/image (452).png" alt=""><figcaption></figcaption></figure>

6. **Import Process**: The importer will replicate workspace details such as name, description, Terraform version, execution mode, variables, tags and the current state into Terrakube.

<figure><img src="../.gitbook/assets/image (453).png" alt=""><figcaption></figcaption></figure>

7. Once the import process is finished, you can view the imported workspaces in Terrakube.
