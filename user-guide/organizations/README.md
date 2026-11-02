# Organizations

Organizations are privately shared spaces for teams to collaborate on infrastructure. Basically are logical containers that you can use to define your company hierarchy to organize the workspaces, modules in the private registry and assign fine grained permissions to your teams.

Like the [Workspaces list](../workspaces/ "mention"), the **Organizations** page has a **New**/**Legacy** view toggle in the top right. The New view is a condensed list showing each organization's workspace count, with a settings shortcut on rows you have permission to manage.

### Set up in this order

For a new deployment, begin with [Production Setup](../../getting-started/production-setup.md) and [Recommended Settings](../../getting-started/recommended-settings.md). Once the services and authentication are configured:

1. [Create an organization](organizations.md).
2. [Create teams and assign permissions](team-management.md), including who may plan, approve and destroy.
3. Prepare [templates](templates/README.md) with the required approval steps. Add [shared variables](variable-collections.md) or [global variables](global-variables.md) if needed.
4. Configure [federated identity](federated-identity.md) for automation, or create [API tokens](api-tokens.md) when needed.
5. Continue to [workspace setup](../workspaces/README.md#workspace-setup-order) to apply access, credentials and run restrictions before the first run.

### Organization topics

{% content-ref url="organizations.md" %}
[organizations.md](organizations.md)
{% endcontent-ref %}

{% content-ref url="global-variables.md" %}
[global-variables.md](global-variables.md)
{% endcontent-ref %}

{% content-ref url="team-management.md" %}
[team-management.md](team-management.md)
{% endcontent-ref %}

{% content-ref url="api-tokens.md" %}
[api-tokens.md](api-tokens.md)
{% endcontent-ref %}

{% content-ref url="federated-identity.md" %}
[federated-identity.md](federated-identity.md)
{% endcontent-ref %}

{% content-ref url="variable-collections.md" %}
[variable-collections.md](variable-collections.md)
{% endcontent-ref %}

{% content-ref url="templates/" %}
[templates](templates/)
{% endcontent-ref %}

{% content-ref url="tags.md" %}
[tags.md](tags.md)
{% endcontent-ref %}
