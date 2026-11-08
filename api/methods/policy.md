# Policy Set

Policy sets, attachments and exemptions drive [Policy Governance](../../user-guide/policy-enforcement-opa.md). They are JSON:API resources under an organization and are also available through the GraphQL endpoint. Creating or changing them requires the **Manage Policies** team permission.

### Policy set (`policy_set`)

| Path | Type | Description |
| --- | --- | --- |
| data.attributes.name | string | Unique name within the organization |
| data.attributes.description | string | OPTIONAL |
| data.attributes.enforcementLevel | string | `hard_mandatory`, `soft_mandatory` or `advisory` |
| data.attributes.shadowEnforcementLevel | string | OPTIONAL. Counted and shown, never blocks |
| data.attributes.overrideTeam | string | Team allowed to override `soft_mandatory` violations |
| data.attributes.global | boolean | Apply to every workspace in the organization |
| data.attributes.repository / branch / folder | string | Git location of the Rego |
| data.attributes.opaVersion | string | OPTIONAL OPA version |
| data.relationships.vcs | object | VCS connection used to clone the repository |

### Policy attachment (`policy_attachment`)

Attaches a non-global policy set to exactly one of `workspace`, `project` or `tag` (relationships), plus `policySet`.

### Policy exemption (`policy_exemption`)

| Path | Type | Description |
| --- | --- | --- |
| data.attributes.ruleId | string | Rule to waive |
| data.attributes.ticketReference | string | Reference for the audit trail |
| data.attributes.justification | string | Why the rule is waived |
| data.attributes.expiresAt | date | OPTIONAL expiry |
| data.relationships.policySet | object | Policy set the rule belongs to |
| data.relationships.workspace / project | object | OPTIONAL scope; omit both for an organization-wide exemption |

Overrides of `soft_mandatory` violations are made from the run page. A run's evaluation results are available as `policy_evaluation` records.

### Supported Operations

{% openapi src="../../.gitbook/assets/terrakube-api.json" path="/policy_set" method="get" %}
{% endopenapi %}

{% openapi src="../../.gitbook/assets/terrakube-api.json" path="/policy_set" method="post" %}
{% endopenapi %}

{% openapi src="../../.gitbook/assets/terrakube-api.json" path="/policy_set/{policy_setId}" method="get" %}
{% endopenapi %}

{% openapi src="../../.gitbook/assets/terrakube-api.json" path="/policy_set/{policy_setId}" method="patch" %}
{% endopenapi %}

{% openapi src="../../.gitbook/assets/terrakube-api.json" path="/policy_set/{policy_setId}" method="delete" %}
{% endopenapi %}
