# Github App

{% hint style="info" %}
This feature is supported from Terrakube 2.23.0
{% endhint %}

For using repositories from GitHub.com with Terrakube workspaces and modules you will need to follow these steps:

{% hint style="info" %}
**Manage VCS Providers** permission is required to perform this action, please check [team-management.md](../organizations/team-management.md "mention") for more info.
{% endhint %}

Navigate to the desired organization and click the **Settings** button, then on the left menu select **VCS Providers**&#x20;

<figure><img src="../../.gitbook/assets/image (308).png" alt=""><figcaption></figcaption></figure>

Select the option for Github App

<figure><img src="../../.gitbook/assets/image (1) (1).png" alt=""><figcaption></figcaption></figure>

In the next page click [https://github.com/settings/apps/new](https://github.com/settings/apps/new) to create a new application adding the required information

<figure><img src="../../.gitbook/assets/image (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

Complete the information like the following:

<figure><img src="../../.gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>

Disable webhook option.

<figure><img src="../../.gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>

Grant the require repository, organization or account permissions and in which type of account you will do the installation.

<figure><img src="../../.gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>

Add "Contents" with "Read-Only"

<figure><img src="../../.gitbook/assets/image (6).png" alt=""><figcaption></figcaption></figure>

Add "Webhooks" with "Read and Write"

<figure><img src="../../.gitbook/assets/image (7).png" alt=""><figcaption></figcaption></figure>

{% hint style="warning" %}
Webhooks  are need it if you will create workspace with webhooks enabled
{% endhint %}

Add "Commit statuses" to "Read and write"

<figure><img src="../../.gitbook/assets/image (8).png" alt=""><figcaption></figcaption></figure>

Finally click "Create Github App"

<figure><img src="../../.gitbook/assets/image (9).png" alt=""><figcaption></figcaption></figure>

After creating the application you need to copy the APP Id to the Terrakube UI

<figure><img src="../../.gitbook/assets/image (12).png" alt=""><figcaption></figcaption></figure>

Scroll down and search for "Generate Private Key" and generate a new private key.

<figure><img src="../../.gitbook/assets/image (11).png" alt=""><figcaption></figcaption></figure>

After downloading the private key we need to to transform the key to PKCS8 using the following command:

```
openssl pkcs8 -topk8 -inform PEM -outform PEM -nocrypt -in my-terrakube-app.private-key.pem -out pkcs8.key
```

{% hint style="warning" %}
The above command assumes the downloaded key from github name is "my-terrakube-app.private-key.pem" and that you want to save the key to a file called "kcs8.key"
{% endhint %}

Now we can add the private key in the Terrakube UI.

<figure><img src="../../.gitbook/assets/image (13).png" alt=""><figcaption></figcaption></figure>

The github app will be added to Terrakube and will be shown like this:

<figure><img src="../../.gitbook/assets/image (14).png" alt=""><figcaption></figcaption></figure>

The final step will be to install the application to your Github account like this.

<figure><img src="../../.gitbook/assets/image (15).png" alt=""><figcaption></figcaption></figure>

And select which repositories will be used with terrakube&#x20;

<figure><img src="../../.gitbook/assets/image (16).png" alt=""><figcaption></figcaption></figure>

The installation should look like this

<figure><img src="../../.gitbook/assets/image (18).png" alt=""><figcaption></figcaption></figure>

Now to use the Github Application when creating a new workspace we just need to add our new connection.

<figure><img src="../../.gitbook/assets/image (19).png" alt=""><figcaption></figcaption></figure>
