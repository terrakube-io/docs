# Cost Estimation

Terrakube can be integrated with Infracost using the Terrakube extension to validate the Terraform plan and estimate the monthly cost of the resources, below you can find an example of how this can be achieved.&#x20;

### Template Definition

The firts step will be to create a Terrakube template that calculate the estimated cost of the resources using infracost and validate the total monthly cost to check if we could deploy the resource or not if the price is bellow 100 USD.&#x20;

```yaml
flow:
- type: "terraformPlan"
  name: "Terraform Plan with Cost Estimation"
  step: 100
  commands:
    - runtime: "GROOVY"
      priority: 100
      after: true
      script: |
        import Infracost

        String credentials = "version: \"0.1\"\n" +
                "api_key: $INFRACOST_KEY \n" +
                "pricing_api_endpoint: https://pricing.api.infracost.io"

        new Infracost().loadTool(
           "$workingDirectory",
           "$bashToolsDirectory", 
           "0.10.12",
           credentials)
        "Infracost Download Completed..."
    - runtime: "BASH"
      priority: 200
      after: true
      script: |
        terraform show -json terraformLibrary.tfPlan > plan.json;
        INFRACOST_ENABLE_DASHBOARD=true infracost breakdown --path plan.json --format json --out-file infracost.json;
        totalCost=$(jq -r '.totalMonthlyCost' infracost.json);
        urlTotalCost=$(jq -r '.shareUrl' infracost.json);
        echo "Total Monthly Cost: $totalCost USD"
        echo "For more detail information please visit: $urlTotalCost"
        if (($totalCost < 100)); 
        then
          echo "The total cost for the resource is below 100 USD. Deployment is approved";
        else
          echo "The total cost for the resource is above 100 USD, cancelling operation";
          exit 1
        fi;
- type: "terraformApply"
  name: "Terraform Apply"
  step: 200
```

In a high level this template will do the following:

* Run the terraform plan for the Terrakube workspace

```yaml
...
flow:
- type: "terraformPlan"
  name: "Terraform Plan with Cost Estimation"
  step: 100
...
```

* After the terraform plan is completed successfully it will import the Infracost inside our job using the Infracost extension.

```yaml
...
    - runtime: "GROOVY"
      priority: 100
      after: true
      script: |
        import Infracost

        String credentials = "version: \"0.1\"\n" +
                "api_key: $INFRACOST_KEY \n" +
                "pricing_api_endpoint: https://pricing.api.infracost.io"

        new Infracost().loadTool(
           "$workingDirectory",
           "$bashToolsDirectory", 
           "0.10.12",
           credentials)
        "Infracost Download Completed..."
...
```

* Once the Infracost has been imported successfully inside our Terrakube Job, you can make use of it to generate the cost estimation with some business rules using a simple BASH script. Based on the result of the Terrakube Job can continue or the execution can be cancelled if the monthly cost is above 100 USD.

```yaml
...
    - runtime: "BASH"
      priority: 200
      after: true
      script: |
        terraform show -json terraformLibrary.tfPlan > plan.json;
        INFRACOST_ENABLE_DASHBOARD=true infracost breakdown --path plan.json --format json --out-file infracost.json;
        totalCost=$(jq -r '.totalMonthlyCost' infracost.json);
        urlTotalCost=$(jq -r '.shareUrl' infracost.json);
        echo "Total Monthly Cost: $totalCost USD"
        echo "For more detail information please visit: $urlTotalCost"
        if (($totalCost < 100)); 
        then
          echo "The total cost for the resource is below 100 USD. Deployment is approved";
        else
          echo "The total cost for the resource is above 100 USD, cancelling operation";
          exit 1
        fi;
...
```

You can use the Terrakube UI to setup the new template and use it across the Terrakube organization.

<figure><img src="../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

### Terraform resources

Now that you have defined the Terrakube template we can define a workspace to test it, for example you can use the following to create some resources in Azure.

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
  location = "West Europe"
}

resource "azurerm_app_service_plan" "example" {
  name                = "api-appserviceplan-pro"
  location            = azurerm_resource_group.example.location
  resource_group_name = azurerm_resource_group.example.name

  sku {
    tier = "Standard"
    size = "S1"
  }
}
```

{% hint style="warning" %}
This will deploy an Azure Service Plan which cost 73 USD every month
{% endhint %}

Lets create a workspace with this information in Terrakube to test the template and run the deployment.

<figure><img src="../.gitbook/assets/image (6).png" alt=""><figcaption></figcaption></figure>

You will have to define the correct credentials to deploy the resource and setup the environment variables and include the infracost key.

<figure><img src="../.gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>

{% hint style="success" %}
Azure Credentials and Infracost key can be define using Global Variables inside the organization so you dont have to define it in every workspace. For more information check [Global Variables](../api/methods/globalvar.md)
{% endhint %}

Now we can start the Terrakube job using the template which include the cost validation.

<figure><img src="../.gitbook/assets/image (4) (5).png" alt=""><figcaption></figcaption></figure>

After a couple of seconds you should be able to see the monthly the terraform plan information and the cost validation.

<figure><img src="../.gitbook/assets/image (12).png" alt=""><figcaption></figcaption></figure>

You can also visit the infracost dashboard to see the cost detail.

<figure><img src="../.gitbook/assets/image (24).png" alt=""><figcaption></figcaption></figure>

Now lets update the terraform resource and use a more expensive one.

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
    tier = "Standard"
    size = "S2"
  }
}
```

{% hint style="warning" %}
This will deploy an Azure Service Plan which cost 146 USD every month
{% endhint %}

Now you can run the workspace again and the deployment will fail because it does not comply with the max cost of 100 USD every month, that you defined in the Terrakube template.

<figure><img src="../.gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>

This is how you can integrate Terrakube with infracost to validate the cost of the resources that you will be deploying and even adding some custom business rules inside the jobs.

