# Publishing Private Modules

{% hint style="info" %}
**Manage Modules** permission is required to perform this action, please check [team-management.md](../organizations/team-management.md "mention") for more info.
{% endhint %}

Click **Registry** in the main menu and then click the **Publish module** button

<figure><img src="../../.gitbook/assets/image (118).png" alt=""><figcaption></figcaption></figure>

Select an existing version control provider or click **Connect to a different VCS** to configure a new one. See [vcs-providers](../vcs-providers/ "mention") for more details.

<figure><img src="../../.gitbook/assets/image (188).png" alt=""><figcaption></figcaption></figure>

Provide the git repository URL and click the **Continue** button.

<figure><img src="../../.gitbook/assets/image (132).png" alt=""><figcaption></figcaption></figure>

In the next screen, configure the required fields and click the **Publish Module** button.&#x20;

<figure><img src="../../.gitbook/assets/image (242).png" alt=""><figcaption></figcaption></figure>

The module will be published inside the specified organization. On the details page, you can view available versions, read documentation, and copy a usage example.

<figure><img src="../../.gitbook/assets/image (191).png" alt=""><figcaption></figcaption></figure>

### Releasing New Versions of a Module <a href="#releasing-new-versions-of-a-module" id="releasing-new-versions-of-a-module"></a>

To release a new version of a module, create a new release tag to its VCS repository. The registry automatically imports the new version.

{% hint style="info" %}
**Tags must be valid SemVer.** The registry strips an optional module-level tag prefix and a leading `v` (so `v2.0.1` and `2.0.1` both publish as version `2.0.1`), then validates what's left as [Semantic Versioning 2.0.0](https://semver.org/). A tag that isn't valid SemVer once those are stripped — `aws-ecs`, `release-42` — is skipped rather than published as a version. The original git tag is preserved alongside the canonical version, so nothing about the tagging convention in your repository has to change.
{% endhint %}

### Deleting Modules <a href="#deleting-versions-and-modules" id="deleting-versions-and-modules"></a>

In the Module details page click the **Delete Module** button and then click the **Yes** button to confirm

<figure><img src="../../.gitbook/assets/image (123).png" alt=""><figcaption></figcaption></figure>
