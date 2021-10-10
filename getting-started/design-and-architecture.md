# 📐 Architecture

This is the high level architecture of the platform

![](../.gitbook/assets/diagrama-sin-titulo.png)

Component descriptions:

* Terrakube API: 
  * Expose a JSON:API or GraphQL API providing endpoints to handle:
    * Organizations.
    * Workspaces
    * Jobs.
    * Modules
    * Providers
* Terrakube Job:
  * Automatic process that check for any pending terraform operations in any workspace (plan, apply or destroy)
* Terrakube Executor:
  * Service that executes the terraform operations, updates the status using the Terrakube API and save the results using different cloud storage providers.
* Terrakube Open Registry:
  * Open Source terraform registry with support for the module and provider protocol.
* Cloud Storage:
  * Cloud storage to save the terraform state and terraform outputs.
* RDBMS:
  * The platform can be used with any database supported by the Liquibase project.
* Security: 
  * To handle authentication the platform uses Azure Active Directory.
* Terrakube CLI:
  * Go based CLI that can communicate with the Terrakube API and execute operation for organizations, workspaces, jobs, modules or providers
* Terrakube UI:
  * React based frontend to handle all Terrakube Operations.
