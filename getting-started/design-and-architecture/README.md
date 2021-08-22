# Design and Architecture

This is the high level architecture of the platform

![](../../.gitbook/assets/diagrama-sin-titulo.png)

Component descriptions:

* AzBuilder API: 
  * Expose a JSON:API or GraphQL API providing endpoints to handle:
    * Organizations.
    * Workspaces
    * Jobs.
    * Modules
    * Providers
* AzBuilder Job:
  * Automatic process that check for any pending terraform operations in any workspace \(plan, apply or destroy\)
* AzBuilder Executor:
  * Service that executes the terraform operations, updates the status using the AzBuilder API and save the results using different cloud storage providers.
* AzBuilder Open Registry:
  * Open Source terraform registry with support for the module and provider protocol.
* Cloud Storage:
  * Cloud storage to save the terraform state and terraform outputs.
* RDBMS:
  * The platform can be used with any database supported by the Liquibase project.
* Security: 
  * To handle authentication the platform uses Azure Active Directory.
* AzBuilder CLI:
  * Go based CLI that can communicate with the AzBuilder API and execute operation for organizations, workspaces, jobs, modules or providers
* AzBuilder UI:
  * React based frontend to handle all AzBuilder Operations.

