# Using Private Modules

All users in an organization with **Manage Modules** permission can view the Terrakube private registry and use the available providers and modules.&#x20;

### Using private modules

Click **Registry** in the main menu. And then click the module you want to use.

<figure><img src="../../.gitbook/assets/image (70) (1).png" alt=""><figcaption></figcaption></figure>

In the module page, select the version in the dropdown list

<figure><img src="../../.gitbook/assets/image (73).png" alt=""><figcaption></figcaption></figure>

You can copy the code reference from the module page

<figure><img src="../../.gitbook/assets/image (71).png" alt=""><figcaption></figcaption></figure>

For example:

```
module "google-network" { 
  source = "8075-azbuilder-terrakube-gmnub6flawx.ws-us89b.gitpod.io/terrakube/google-network/google" 
  version = "v6.0.1" 
  # insert required variables here 
}
```

### **Submodules**

If your repository has submodules, Terrakube will scan the **modules** folder to identify all the submodules. Then, you will see a dropdown list with the submodules in the UI.

<figure><img src="../../.gitbook/assets/image (84).png" alt=""><figcaption></figcaption></figure>

To view the readme file, inputs, outputs, resources and other information for any submodule, you can choose the submodule you want.

<figure><img src="../../.gitbook/assets/image (85).png" alt=""><figcaption></figcaption></figure>

You can also copy the details of how to configure any submodule, just like the main module. Example:

```
module "iam" { 
  source = "8075-azbuilder-terrakube-7qovhyoq3u9.ws-eu105.gitpod.io/aws/iam/aws//modules/iam-account" 
  version = "v5.30.0" 
  # insert required variables here 
}
```

In the submodule view you can switch to the differents submodules.

<figure><img src="../../.gitbook/assets/image (86).png" alt=""><figcaption></figcaption></figure>

Or you can back to the main module with the **Back** button on the top of the page.

<figure><img src="../../.gitbook/assets/image (87).png" alt=""><figcaption></figcaption></figure>

### **Configure Authentication**

When running Terraform on the CLI, you must configure credentials in [.terraformrc or terraform.rc ](https://developer.hashicorp.com/terraform/cli/config/config-file)to access the private modules.&#x20;

For example:

```
credentials "8075-azbuilder-terrakube-gmnub6flawx.ws-us89b.gitpod.io" { 
  # valid user API token:
  token = "xxxxxx.yyyyyy.zzzzzzzzzzzzz"
}
```

To get the API token you can check [api-tokens.md](../organizations/api-tokens.md "mention").

As an alternative you can run the [terraform login](https://developer.hashicorp.com/terraform/cli/commands/login) command to obtain and save an user API token.&#x20;

```
terraform login [terrakube hostname]
```

