# Azure Storage Account

{% hint style="info" %}
This guide will assume that you are using the minikube deployment, but the storage backend can be used in any real kubernetes environment.
{% endhint %}

The first step will be to create one azure storage account with the following containers:

* content
* registry
* tfoutput
* tfstate

{% hint style="info" %}
Create Azure Storage Account tutorial [link](https://learn.microsoft.com/en-us/azure/storage/common/storage-account-create?tabs=azure-portal)
{% endhint %}

Once the storage account is created you will have to get the "[Access Key](https://learn.microsoft.com/en-us/azure/storage/common/storage-account-keys-manage?tabs=azure-portal)"

Now you have all the information we will need to create a terrakube.yaml for our terrakube deployment with the following content:

```yaml
## Terrakube Storage
storage:
  defaultStorage: false
  azure:
    storageAccountName: "<<STORAGE ACCOUNT NAME>>"
    storageAccountResourceGroup: "<< RESOURCE GROUP >>"
    storageAccountAccessKey: "<< STORAGE ACCOUNT ACCESS KEY"
```

Now you can install terrakube using the command

```bash
helm install --values terrakube.yaml terrakube terrakube-repo/terrakube -n terrakube
```

