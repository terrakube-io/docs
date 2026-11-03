# VCS Providers

Terrakube empowers collaboration between different teams in your organization. To achieve this, you can integrate Terrakube with your version control system (VCS) provider. Although Terrakube can be used with public Git repositories or with private Git repositories using SSH, connecting to your Terraform code through a VCS provider is the preferred method. This allows for a more streamlined and secure workflow, as well as easier management of access control and permissions.

Terrakube supports the following VCS providers:

{% content-ref url="github.com.md" %}
[github.com.md](github.com.md)
{% endcontent-ref %}

{% content-ref url="github-enterprise.md" %}
[github-enterprise.md](github-enterprise.md)
{% endcontent-ref %}

{% content-ref url="gitlab.com.md" %}
[gitlab.com.md](gitlab.com.md)
{% endcontent-ref %}

{% content-ref url="gitlab-ee-and-ce.md" %}
[gitlab-ee-and-ce.md](gitlab-ee-and-ce.md)
{% endcontent-ref %}

{% content-ref url="bitbucket.com.md" %}
[bitbucket.com.md](bitbucket.com.md)
{% endcontent-ref %}

{% content-ref url="azure-devops.md" %}
[azure-devops.md](azure-devops.md)
{% endcontent-ref %}

{% content-ref url="ssh.md" %}
[ssh.md](ssh.md)
{% endcontent-ref %}

### Webhooks <a href="#webhooks" id="webhooks"></a>

Terrakube uses webhooks to monitor new commits. This feature is not available for SSH-based repositories. Azure DevOps is supported via inbound service hooks by default, with an outbound polling fallback for private networks that the Azure DevOps cloud can't reach — see [azure-devops.md](azure-devops.md#webhooks "mention").

* When new commits are added to a branch, Terrakube workspaces based on that branch will automatically initiate a Terraform job. Terrakube will use the "Plan and apply" template by default, but you can specify a different Template during the Workspace creation.
* When you specify a directory in the Workspace. Terrakube will run the job only if a file changes in that directory
* For GitHub, GitLab, and Bitbucket, pull requests can also get plan/apply results posted as comments — see [webhooks.md](../workspaces/webhooks.md "mention") for the full per-workspace webhook configuration.
