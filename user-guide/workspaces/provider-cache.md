# Provider Cache

The component use to execute terraform/opentofu configuration is stateless so everytime that it is executing a job will try to download the terraform/opentofu providers, in order to speed up the execution the following environment variable can be added to have a provider cache.

```
TF_PLUGIN_CACHE_DIR=/home/cnb/.terraform.d/plugin-cache
```

<figure><img src="../../.gitbook/assets/image (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

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

When running tofu init, temporary files such as .terraform.lock.hcl are generated based on the current machine’s architecture. On subsequent runs, OpenTofu verifies that the required providers match the architecture specified in the lock file to decide whether a new download is necessary.

However, when working locally on macOS, the architecture (e.g., darwin/amd64 or darwin/arm64) differs from the architecture used by remote Terrakube jobs (linux/arm64). This mismatch forces OpenTofu to redownload providers each time.

If you want to avoid that issue you need to run this command to generate lock file for Terrakube's image architecture.

```
tofu providers lock -platform=linux_amd64
```

{% hint style="warning" %}
Make sure to use the correct size for the volume mount, in the example is only using 1024Mi, more information can be found in this [link](https://developer.hashicorp.com/terraform/cli/config/config-file#provider-plugin-cache)
{% endhint %}

{% hint style="warning" %}
If you want to use cache with EphemeralJob you will need to provision a volume with in ReadWriteMany as several jobs can be writing to the cache at the same time.
{% endhint %}
