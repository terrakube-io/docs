# Azure Monitor

### Description

The Azure Monitor Metrics action allows users to fetch and visualize metrics from Azure Monitor for a specified resource.&#x20;

### Display Criteria

By default, this Action is disabled and will appear for all the `azurerm` resources. If you want to display this action only for certain resources, please check  [display criteria](terrakube.azure-monitor.md#display-criteria).&#x20;

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

* Click the **Monitor** tab to view metrics for the selected resource.

<figure><img src="../../../../.gitbook/assets/image (2) (1).png" alt=""><figcaption></figcaption></figure>

* You can view additional details for the metrics using the tooltip.

<figure><img src="../../../../.gitbook/assets/image (3) (1).png" alt=""><figcaption></figcaption></figure>

* You can see more details for each chart by navigating through it.

<figure><img src="../../../../.gitbook/assets/image (4) (1).png" alt=""><figcaption></figcaption></figure>
