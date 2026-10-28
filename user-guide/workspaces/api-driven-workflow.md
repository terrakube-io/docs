# API-driven Workflow

Anything the UI does can be done over the [API](../../api/getting-started.md), so you can start runs from your own pipeline or tooling. For GitHub Actions, see [CI/CD Integration](../ci-cd-integration/README.md); for the Terraform CLI, see [CLI-driven Workflow](cli-driven-workflow.md).

### Start a run

Create a `job` for a workspace with the template to run:

```bash
curl -X POST "$TERRAKUBE_API/api/v1/organization/$ORG_ID/job" \
  -H "Authorization: Bearer $TERRAKUBE_TOKEN" \
  -H "Content-Type: application/vnd.api+json" \
  -d '{
    "data": {
      "type": "job",
      "attributes": {
        "templateReference": "<template id>",
        "overrideBranch": "main"
      },
      "relationships": {
        "workspace": { "data": { "type": "workspace", "id": "<workspace id>" } }
      }
    }
  }'
```

Creating a job needs permission to plan in the workspace. Poll the job (`GET .../job/{id}`) or subscribe to status changes until it reaches `waitingApproval`, `completed` or a failure.

### Variables for one run

Pass `variables` on the job to set Terraform input variables for that run only, without touching the workspace's stored variables. This is how CI can forward values such as `TF_VAR_image_tag`:

```json
"attributes": {
  "templateReference": "<template id>",
  "variables": { "image_tag": "release-42" }
}
```

* A run variable replaces a workspace or template variable with the same name. `-var` and variable files keep Terraform's normal precedence.
* Values are encrypted, never returned by the API, and kept with the run so a saved-plan apply uses the same values. The run page lists the variable **names**.
* Up to 100 variables and 64 KiB in total. Names must be valid Terraform variable names.
* You need rights to manage the workspace (the same as editing its variables). A plan-only user can create jobs but not send `variables`.
* The plan file contains input values, as it does for workspace variables. Mark secrets `sensitive = true`.

### Approve or discard

A run waiting for approval is approved by updating the job's status to `approved`, or discarded with `rejected`, by a user in the template's approval team. Terrakube records who approved the run and when.

### Large lists

Page through collections with `page[number]` and `page[size]`. The workspaces query on the GraphQL endpoint (`/graphql/api/v1`) also supports cursor pagination, which stays fast on organizations with thousands of workspaces.
