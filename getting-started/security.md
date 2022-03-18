# 🔐 Security

### General security

Terrakube security is based organizations and groups&#x20;

A group inside Terrakube can have two types of members:

* Azure Active Directory users
* Azure Active Directory applications

An organization can have one or multiple groups and each group have different kind of access to manage the following options:

* Module
  * Manage terraform modules inside an organization
* VCS
  * Manage private connections to different VCS like Github, Bitbucket, Azure DevOps and Gitlab.
* Template
  * Manage the custom flows written in Terrakube Configuration Language when running any job inside the platform
* Workspaces
  * Manage the terraform workspaces to run remote terraform operations.&#x20;
* Providers
  * Manage the terraform providers available inside the platform

{% hint style="info" %}
Adding a group to an organization will grant access to read the content inside the organization but to be able to manage any option like module, workspace, templates or providers or VCS a Terrakube administrator will need to grant it&#x20;
{% endhint %}

### Administrator groups

There is one special group inside Terrakube called "TERRAKUBE\_ADMINiSTRATORS", this is the only group that has access to create organizations and grant access to a teams to manage differente organization features.





###
