# Azure Monitor

### Description

The Azure Monitor Metrics action allows users to fetch and visualize metrics from Azure Monitor for a specified resource.&#x20;

### Display Criteria

By default, this Action is disabled and when enabled will appear for all the `azurerm` resources. If you want to display this action only for certain resources, please check  [display criteria](terrakube.azure-monitor.md#display-criteria).&#x20;

### Setup

This action requires the following variables as [Workspace Variables](../../variables.md#workspace-specific-variables) or [Global Variables](../../../organizations/global-variables.md) in the Workspace Organization:

* `ARM_CLIENT_ID`: The Azure Client ID used for authentication.&#x20;
* `ARM_CLIENT_SECRET`: The Azure Client Secret used for authentication.
* `ARM_TENANT_ID`: The Azure Tenant ID associated with your subscription.
* `ARM_SUBSCRIPTION_ID`: The Azure Subscription ID where the VM is located.

{% hint style="info" %}
The Client ID should have at least **Monitoring Reader** or **Reader** access on resource group or subscription.
{% endhint %}

### Usage

* Navigate to the `Workspace Overview` or the `Visual State` and click on a resource name.

<figure><img src="../../../../.gitbook/assets/image (470).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../../.gitbook/assets/image (471).png" alt=""><figcaption></figcaption></figure>

* Click the **Monitor** tab to view metrics for the selected resource.

<figure><img src="../../../../.gitbook/assets/image (32).png" alt=""><figcaption></figcaption></figure>

* You can view additional details for the metrics using the tooltip.

<figure><img src="../../../../.gitbook/assets/image (33).png" alt=""><figcaption></figcaption></figure>

* You can see more details for each chart by navigating through it.

<figure><img src="../../../../.gitbook/assets/image (34).png" alt=""><figcaption></figcaption></figure>
