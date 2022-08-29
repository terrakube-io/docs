# 🔐 Security

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

### Administrator group

There is one special group inside Terrakube called _**TERRAKUBE\_ADMIN**_, this is the only group that has access to create organizations and grant access to a teams to manage different organization features, you can also customize the group name if you want to use a different name depending on which [Dex connector](https://dexidp.io/docs/connectors/) you are using when running Terrakube.

###
