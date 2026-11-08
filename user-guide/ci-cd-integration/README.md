# CI/CD Integration

Run Terrakube from your pipelines, or let Terrakube run on its own from your repository. The options:

| CI/CD tool | Integration |
| --- | --- |
| GitHub | The [Terrakube GitHub Action](github-actions.md#the-terrakube-github-action) (`terrakube-io/terrakube-action-github`), the [Terraform or OpenTofu CLI](github-actions.md#the-terraform-or-opentofu-cli), or the [Terrakube CLI](github-actions.md#the-terrakube-cli) |
| Bitbucket | The [Bitbucket Pipe](bitbucket.md) |
| Any other tool | The [Terraform or OpenTofu CLI](../workspaces/cli-driven-workflow.md), the [Terrakube CLI](../terrakube-cli/README.md), or the [API](../../api/getting-started.md) |

You can also skip the pipeline: connect a [VCS provider](../vcs-providers/README.md) and a [webhook](../workspaces/webhooks.md) and Terrakube starts runs on pushes and pull requests itself.

Pipelines can authenticate without storing a Terrakube token. See [Federated OIDC Authentication](federated-oidc-authentication.md) for GitHub Actions and Kubernetes examples that use short-lived identity tokens.
