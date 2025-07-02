# SSH

Terrakube can use SSH keys to authenticate to your private repositories. The SSH keys can be uploaded inside the settings menu.

<figure><img src="../../.gitbook/assets/image (240).png" alt=""><figcaption></figcaption></figure>

To handle SSH keys inside Terrakube your team should have "Manage VCS settings" access

<figure><img src="../../.gitbook/assets/image (236).png" alt=""><figcaption></figcaption></figure>

Terrakube support keys with RSA and ED25519

<figure><img src="../../.gitbook/assets/image (170).png" alt=""><figcaption></figcaption></figure>

SSH keys can be used to authenticate to private repositories where you can store modules or workspaces

Once SSH keys are added inside your organization you can use them like the following example:

### Modules:

<figure><img src="../../.gitbook/assets/image (320).png" alt=""><figcaption></figcaption></figure>

### Workspace:

<figure><img src="../../.gitbook/assets/image (225).png" alt=""><figcaption></figcaption></figure>

{% hint style="warning" %}
When using SSH keys make sure to use your repository URL using the ssh format. For github it is something like [_**git@github.com**_](mailto:git@github.com)_**:AzBuilder/terrakube-docker-compose.git**_
{% endhint %}

### SSH Key Injection

The selected SSH key will be used to clone the workspace information at runtime when running the job inside Terrakube, but it will also be injected to the job execution to be use inside our terraform code.&#x20;

For example if you are using a module using a GIT connection like the following:

```
module "test" {
  source = "git@bitbucket.org:alfespa17/private-module.git"
}
```

When running the job, internally terraform will be using the selected SSH key to clone the necesary module dependencies like the below image:

<figure><img src="../../.gitbook/assets/image (216).png" alt=""><figcaption></figcaption></figure>

When using a VCS connection you can select wich SSH key should be injected when downloading modules in the workspace settings:

<figure><img src="../../.gitbook/assets/image (64).png" alt=""><figcaption></figcaption></figure>

This will allow to use modules using the git format inside a workspace with VCS connection like the following:

```
module "test" {
  source = "git@github.com:alfespa17/simple-terraform.git"
}
```
