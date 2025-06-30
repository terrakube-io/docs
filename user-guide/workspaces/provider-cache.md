# Provider Cache

The component use to execute terraform/opentofu configuration is stateless so everytime that it is executing a job will try to download the terraform/opentofu providers, in order to speed up the execution the following environment variable can be added to have a provider cache.

```
TF_PLUGIN_CACHE_DIR=/home/cnb/.terraform.d/plugin-cache
```

<figure><img src="../../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

When deploying the helm chart the following can be used to add an emptyDir volume to cache the providers.

```
executor:
  volumes:
    - name: cache-volume
      emptyDir:
        sizeLimit: 1024Mi
  volumeMounts:
  - mountPath: /home/cnb/.terraform.d/plugin-cache
    name: cache-volume
```

{% hint style="warning" %}
Make sure to use the correct size for the volume mount, in the example is only using 1024Mi, more information can be found in this [link](https://developer.hashicorp.com/terraform/cli/config/config-file#provider-plugin-cache)
{% endhint %}
