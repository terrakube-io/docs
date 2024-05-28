# Restart Azure VM

### Description&#x20;

The Restart VM action is designed to restart a specific virtual machine in Azure. By using the context of the current state, this action fetches an Azure access token and issues a restart command to the specified VM. The action ensures that the VM is restarted successfully and provides feedback on the process.

### Display Criteria

By default, this Action is disabled and when enabled will appear for all resources that have resource type `azurerm_virtual_machine`. If you want to display this action only for certain resources, please check [display criteria](restart-azure-vm.md#display-criteria).

### Setup

This action requires the following variables as [Workspace Variables](../../variables.md#workspace-specific-variables) or [Global Variables](../../../organizations/global-variables.md) in the Workspace Organization:

* `ARM_CLIENT_ID`: The Azure Client ID used for authentication.&#x20;
* `ARM_CLIENT_SECRET`: The Azure Client Secret used for authentication.
* `ARM_TENANT_ID`: The Azure Tenant ID associated with your subscription.
* `ARM_SUBSCRIPTION_ID`: The Azure Subscription ID where the VM is located.

{% hint style="info" %}
The Client ID should have at least Virtual Machine Contributor access on the VM or resource group.
{% endhint %}

### Usage

* Navigate to the `Workspace Overview` or the `Visual State` and click on a resource name.

<figure><img src="../../../../.gitbook/assets/image (393).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../../.gitbook/assets/image (394).png" alt=""><figcaption></figcaption></figure>

* In the Resource Drawer, click the "Restart" button.

<figure><img src="../../../../.gitbook/assets/image (18).png" alt=""><figcaption></figcaption></figure>

* The VM will be restarted, and a success or error message will be displayed.

<figure><img src="../../../../.gitbook/assets/image (1) (1) (1).png" alt=""><figcaption></figcaption></figure>
