# Security

### General security

Terrakube security is based organizations and groups.

{% hint style="info" %}
All Dex connectors that implement the groups claims can be used inside Terrakube.
{% endhint %}

An organization can have one or multiple groups and each group have different kind of access to manage the following options:

* Module
  * Manage terraform modules inside an organization
* VCS
  * Manage private connections to different VCS like Github, Bitbucket, Azure DevOps and Gitlab and handle SSH keys
* Template
  * Manage the custom flows written in Terrakube Configuration Language when running any job inside the platform
* Workspaces
  * Manage the terraform workspaces to run remote terraform operations.
* Providers
  * Manage the terraform providers available inside the platform

{% hint style="warning" %}
Adding a group to an organization will grant access to read the content inside the organization but to be able to manage any option like module, workspace, templates or providers or VCS a Terrakube administrator will need to grant it
{% endhint %}

### Build supply-chain safeguards

Terrakube's release build pipeline includes safeguards that make its container build inputs and outputs easier to audit:

* **OpenSSF Scorecard** runs security checks against the source repository.
* **Syft** generates a software bill of materials (SBOM) during container builds.
* Container base images are pinned by digest, rather than a mutable tag, to make builds reproducible.

These controls are part of the Terrakube build and release process; they do not require configuration in a deployed Terrakube instance.

### Administrator group

There is one special group inside Terrakube called _**TERRAKUBE\_ADMIN**_, this is the only group that has access to create organizations and grant access to a teams to manage different organization features, you can also customize the group name if you want to use a different name depending on which [Dex connector](https://dexidp.io/docs/connectors/) you are using when running Terrakube.


### Recommended controls

Start with [Recommended Settings](recommended-settings.md) for production workspace restrictions, run permissions, token policy and cloud trust. [Run Restrictions](../user-guide/workspaces/run-restrictions.md) explains the 2.34.0 workspace controls; [Token Security](deployment/token-security.md) covers issuance, revocation and Dex audiences.

In 2.34.0, API checks also enforce organization ownership across jobs, templates, workspace relationships and variable collections. A valid object ID from another organization does not grant access to it. Integrations must use resources belonging to the same organization.
