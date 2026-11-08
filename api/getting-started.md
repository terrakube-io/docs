# Getting started

Everything in the Terrakube UI is available through the API, so you can automate it from scripts, pipelines and your own tools. The API has two forms:

| Endpoint | Use it for |
| --- | --- |
| `/api/v1` | REST, in [JSON:API](https://jsonapi.org/) format. Create, read, update and delete everything. |
| `/graphql/api/v1` | GraphQL over the same data. Good for reading nested data in one request. |

Both are generated from the same model and enforce the same permissions as the UI. In the examples, `$API` is your API URL, for example `https://terrakube-api.example.com`, and `$TOKEN` is a token.

### Authenticate

Send a bearer token on every request:

```bash
export API=https://terrakube-api.example.com
export TOKEN=<token>

curl -H "Authorization: Bearer $TOKEN" "$API/api/v1/organization?fields[organization]=name"
```

Pick the token that fits:

| Token | Use it for |
| --- | --- |
| [Personal access token](../user-guide/organizations/api-tokens.md) | Your own scripts. Acts as you. |
| [Team token](../user-guide/organizations/api-tokens.md) | Shared automation. Has the team's permissions. |
| [Federated OIDC token](../user-guide/organizations/federated-identity.md) | CI systems such as GitHub Actions, with no stored secret. |
| `terraform login` token | The Terraform CLI. See [CLI-driven Workflow](../user-guide/workspaces/cli-driven-workflow.md#terraform-login). |

A request without a valid token returns `401`. A token without permission returns `403`.

### Read data

List and find things with filters, sorting, paging and relationships:

```bash
# Workspaces whose name starts with "prod", 20 per page, with totals
curl -g -H "Authorization: Bearer $TOKEN" \
  "$API/api/v1/organization/$ORG_ID/workspace?filter[workspace]=name==prod*&sort=name&page[size]=20&page[totals]&fields[workspace]=name"

# One run with its steps
curl -g -H "Authorization: Bearer $TOKEN" \
  "$API/api/v1/organization/$ORG_ID/job/$JOB_ID?include=step"

# Runs waiting for approval
curl -g -H "Authorization: Bearer $TOKEN" \
  "$API/api/v1/organization/$ORG_ID/job?filter[job]=status==waitingApproval"
```

| Parameter | Meaning |
| --- | --- |
| `filter[type]=expression` | [RSQL](https://github.com/jirutka/rsql-parser) filter such as `name==prod*`, `status=in=(failed,cancelled)` or `a==1;b==2` (and). A comma between expressions means OR. |
| `sort=name`, `sort=-name` | Ascending or descending. |
| `page[size]`, `page[number]`, `page[totals]` | Paging, and a `meta` block with the total. |
| `include=step` | Return related records in the same response. |
| `fields[type]=a,b` | Return only some attributes. |

Add `-g` to `curl` so it does not treat square brackets as ranges.

The same read as GraphQL:

```bash
curl -H "Authorization: Bearer $TOKEN" -H "Content-Type: application/json" \
  -d '{"query":"{ organization { edges { node { name workspace { edges { node { name } } } } } } }"}' \
  "$API/graphql/api/v1"
```

### Write data

Writes use `Content-Type: application/vnd.api+json` and a JSON:API body with `type`, `attributes` and `relationships`. For example, start a run:

```bash
curl -X POST -H "Authorization: Bearer $TOKEN" -H "Content-Type: application/vnd.api+json" \
  "$API/api/v1/organization/$ORG_ID/job" -d '{
    "data": {
      "type": "job",
      "attributes": { "templateReference": "'$TEMPLATE_ID'" },
      "relationships": {
        "workspace": { "data": { "type": "workspace", "id": "'$WORKSPACE_ID'" } }
      }
    }
  }'
```

Use `PATCH` to update, with the record `id` in the body, and `DELETE` to remove it. A `DELETE` must not carry a body or a content type.

See [API-driven Workflow](../user-guide/workspaces/api-driven-workflow.md) for the full run lifecycle from the API: start, wait, approve, and pass run-specific variables.

### Reference

* The [method pages](methods/README.md) document each resource with its fields and operations.
* The complete OpenAPI specification is [terrakube-api.json](../.gitbook/assets/terrakube-api.json). Your own API serves an always-current copy at `$API/doc`, and the UI shows an interactive version under **Help & Support → API Docs**, signed in as you.
* Other ways to automate Terrakube: the [Terrakube CLI](../user-guide/terrakube-cli/README.md), the [Terraform provider](https://registry.terraform.io/providers/terrakube-io/terrakube/latest/docs), a [Go client](https://github.com/terrakube-io/terrakube-go) and a [Java client](https://github.com/terrakube-io/terrakube-spring-boot-starter).
