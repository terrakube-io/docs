---
description: Setup Terrakube using SQL Azure and Azure Storage Account
---

# Azure Kubernetes Service (AKS)

To use Terrakube inside a AKS with SQL Azure and Azure Storage for persisten storage please use the following steps:

### Step 1 - Azure Active Directory Application

To register the Active Directory Application please use the following [terraform module.](https://github.com/AzBuilder/terraform-azurerm-terrakube-app-registration)

### Step 2 - Azure SQL Azure

To create a SQL Azure Database please refer to the following Microsoft [guide](https://docs.microsoft.com/en-us/azure/azure-sql/database/single-database-create-quickstart?tabs=azure-portal).

### Step 3 - Create Azure Storage Account

To create the azure storage account please refer to the following [terraform module](https://github.com/AzBuilder/terraform-azurerm-terrakube-cloud-storage).

### Step 4 - Setup Let's Encrypt and Nginx Ingress

To setup an Ngnix Ingress Controller you can use the Microsoft guide:

{% embed url="https://docs.microsoft.com/en-us/azure/aks/ingress-basic?tabs=azure-cli" %}

To setup Azure Kubernetes Service with Lets Encrypt please refer to the followings guides:

{% embed url="https://docs.microsoft.com/en-us/azure/aks/ingress-tls" %}

{% embed url="https://docs.microsoft.com/en-us/azure/aks/ingress-static-ip" %}

### Step 5 - Deploy Terrakube with Helm.

Use the helm chart to deploy Terrakube in the cluster.

{% embed url="https://github.com/AzBuilder/terrakube-helm-chart" %}

{% hint style="warning" %}
We are currently working to automate all the steps
{% endhint %}
