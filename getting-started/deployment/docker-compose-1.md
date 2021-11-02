---
description: H2 In-Memory Database + Azure Storage
---

# Docker Compose + Azure Storage

### Requirements.

In order to use the docker compose with Azure Active Directory authentication and  Azure Storage Account please create the following resources:

1. Azure Active Directory Application
2. Azure Storage Account

### Step 1 - Register Azure Application

You can use the [terraform module](https://github.com/AzBuilder/terraform-azurerm-terrakube-app-registration) to simplify the creation of the Azure Active Directory Application.

### Step 2 - Create Azure Storage Account

Create an Azure Storage account and add the following containers with private access:

| tfoutput | Container to store terraform output |
| -------- | ----------------------------------- |
| tfstate  | Container to store terraform state  |

## Step 3 - Docker Compose Environment Variables

Fill environment variables for api-server inside docker-compose.yaml:

| Component    | Description                   |
| ------------ | ----------------------------- |
| AzureAdAppId | Azure Application (Client Id) |

Fill environment variables for api-job inside docker-compose.yaml:

| Component              | Description                   |
| ---------------------- | ----------------------------- |
| AzureAdAppClientId     | Azure application (Client Id) |
| AzureAdAppClientSecret | Azure application secret      |
| AzureAdAppTenantId     | Azure tenant Id               |

Fill environment variables for terraform executor inside docker-compose.yaml:

| Component                             | Description                          |
| ------------------------------------- | ------------------------------------ |
| AzureTerraformStateResourceGroup      | Azure storage account resource group |
| AzureTerraformStateStorageAccountName | Azure storage account name           |
| AzureTerraformStateStorageAccessKey   | Azure storage account access key     |
| AzureTerraformOutputAccountName       | Azure storage account name           |
| AzureTerraformOutputAccountKey        | Azure storage account key            |
| AzureAdAppClientId                    | Azure application (Client Id)        |
| AzureAdAppClientSecret                | Azure application secret             |
| AzureAdAppTenantId                    | Azure tenant Id                      |

> You can use one the same storage account for the terraform state and output or you can use different storage accounts

## Step 4 - Run AzBuilder Platform

```bash
docker-compose up
```
