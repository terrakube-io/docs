# SSH

Terrakube can use SSH keys to authenticate to your private repositories. The SSH keys can be uploaded inside the settings menu.

<figure><img src="../../.gitbook/assets/image (1) (1).png" alt=""><figcaption></figcaption></figure>

To handle SSH keys inside Terrakube your team should have "Manage VCS settings" access

<figure><img src="../../.gitbook/assets/image (26).png" alt=""><figcaption></figcaption></figure>

Terrakube support keys with RSA and ED25519

<figure><img src="../../.gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>

SSH keys can be used to authenticate to private repositories where you can store modules or workspaces

Once SSH keys are added inside your organization you can use them like the following example:

### Modules:

<figure><img src="../../.gitbook/assets/image (6).png" alt=""><figcaption></figcaption></figure>

### Workspace:

<figure><img src="../../.gitbook/assets/image (14).png" alt=""><figcaption></figcaption></figure>

{% hint style="warning" %}
When using SSH keys make sure to use your repository URL using the ssh format. For github it is something like [_**git@github.com**_](mailto:git@github.com)_**:AzBuilder/terrakube-docker-compose.git**_
{% endhint %}
