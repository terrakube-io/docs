# Introduction

Terrakube is an open source collaboration platform for running remote infraestructure as code operations using terraform that aims to be a complete replacement for close source tools like  Terraform Enterprise and Scalr.

The platform can be easily installed in any kubernetes cluster, you can easily customize the platform using other open source tools already available for terraform and you can integrate the tool with Azure Active Directory to handle authentication and authorization.&#x20;

Terrakube provides different features like the following:

* Terraform module protocol:
  * This allows Terrakube to expose an open source terraform registry that is protect is private by default and it is protected with Azure Active Director authentication.
* Terraform provider protocol:
  * This allows Terrakube to expose an open source terraform registry where you can have create your own private terraform providers or create a mirror of the current terraform providers available in the public Hashicorp registry.
* Handle you infrastructure as code using Organization like closed source tools like Terraform Enterprise and Scalr.
* Easily extended by default integrating with many open source tools available today for terraform CLI.
* Support to run the platform using any **RDBMS** supported by [liquibase](https://www.liquibase.org) (mysql, postgres, oracle, sql server, etc).
* Plugins support to extends functionality using different cloud providers and open source tools using the Terrakube Configuration Languague (TCL).
* Easily extends the platform using common technologies like react js, java, spring, elide, liquidbase, groovy, bash, go.
* Native integration with the most popular VCS like:
  * Github.
  * Azure DevOps
  * Bitbucket
  * Gitlab
* Extends and create custom flows when running terraform remote operations with custom logics like:
  * Approvals
  * Budget reviews
  * Security reviews
  * Custom logic that you can build with groovy and bash scripts

