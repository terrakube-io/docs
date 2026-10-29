# GitHub Actions

There are three ways to run Terrakube from GitHub Actions. Pick the one that fits how you work:

| Approach | Best for |
| --- | --- |
| [Terrakube GitHub Action](#the-terrakube-github-action) | A repository where each folder is a workspace. Plans on pull requests, applies on merge, with results as PR comments. |
| [Terraform or OpenTofu CLI](#the-terraform-or-opentofu-cli) | You already run `terraform plan` and `apply` in a workflow and want the run to execute in Terrakube. |
| [Terrakube CLI](#the-terrakube-cli) | Scripting: trigger runs, and manage organizations, workspaces, variables and more. |

All three can use a Personal Access Token or team token stored as a secret. To avoid storing a long-lived token, use [Federated OIDC Authentication](federated-oidc-authentication.md).

### The Terrakube GitHub Action

[`terrakube-io/terrakube-action-github`](https://github.com/terrakube-io/terrakube-action-github) maps your repository onto Terrakube: the repository is an organization and **each folder containing a `terrakube.json` file is a workspace**. Workspaces are created for you, and on a pull request only the folders that changed are run.

Pin the action to a release tag, for example `@1.1.0` ([releases](https://github.com/terrakube-io/terrakube-action-github/releases)), rather than `@main`.

#### Plan on pull requests

```yaml
# .github/workflows/pull_request.yml
name: Terrakube Plan
on:
  pull_request:
    types: [opened, synchronize, reopened]

jobs:
  plan:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
        with:
          fetch-depth: 0

      - uses: terrakube-io/terrakube-action-github@1.1.0
        with:
          TERRAKUBE_TOKEN: ${{ secrets.TERRAKUBE_PAT }}
          TERRAKUBE_ENDPOINT: ${{ secrets.TERRAKUBE_ENDPOINT }}
          TERRAKUBE_ORGANIZATION: "my-organization"
          TERRAKUBE_TEMPLATE: "Terraform-Plan"
          TERRAKUBE_BRANCH: ${{ github.head_ref }}
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
          SHOW_OUTPUT: true
```

#### Apply on merge

```yaml
# .github/workflows/push_main.yml
name: Terrakube Apply
on:
  push:
    branches: [main]

jobs:
  apply:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
        with:
          fetch-depth: 0

      - uses: terrakube-io/terrakube-action-github@1.1.0
        with:
          TERRAKUBE_TOKEN: ${{ secrets.TERRAKUBE_PAT }}
          TERRAKUBE_ENDPOINT: ${{ secrets.TERRAKUBE_ENDPOINT }}
          TERRAKUBE_ORGANIZATION: "my-organization"
          TERRAKUBE_TEMPLATE: "Terraform-Plan/Apply"
          TERRAKUBE_BRANCH: "main"
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
          SHOW_OUTPUT: false
```

`TERRAKUBE_TEMPLATE` is the name of a [template](../organizations/templates/README.md) in your organization.

#### Configure each workspace

Add a `terrakube.json` file to each folder:

```json
{
  "workspace": "network",
  "terraform": "1.9.8"
}
```

#### Inputs and outputs

| Input | Description |
| --- | --- |
| `TERRAKUBE_TOKEN` (required) | Personal access token or team token. |
| `TERRAKUBE_ENDPOINT` (required) | URL of the Terrakube API. |
| `TERRAKUBE_ORGANIZATION` (required) | Organization to use. |
| `TERRAKUBE_TEMPLATE` (required) | Template to run. |
| `TERRAKUBE_BRANCH` (required) | Branch the run uses. Defaults to `main`. |
| `GITHUB_TOKEN` (required) | Used to post results on the pull request. |
| `SHOW_OUTPUT` (required) | Show the Terrakube logs in PR comments. |
| `TERRAKUBE_SSH_KEY_NAME` | Name of an [SSH key](../vcs-providers/ssh.md) in the organization, for private repositories. |
| `IGNORE_SSL_ERROR` | Ignore TLS errors, for example a self-signed certificate. Default `false`. |

The action outputs `JOB_ID`, the id of the Terrakube run.

Put cloud credentials in [Global Variables](../organizations/global-variables.md) of the organization rather than in the workflow, and Terraform variables in a `terraform.tfvars` in each folder or in the workspace.

{% hint style="info" %}
Terrakube 2.34 adds [run-specific variables](../workspaces/api-driven-workflow.md#variables-for-one-run) so CI can pass `TF_VAR_*` values to a single run. Check the action's release notes for support and for compatibility before you upgrade Terrakube.
{% endhint %}

### The Terraform or OpenTofu CLI

Terrakube works as a remote backend for the Terraform and OpenTofu CLI (the [CLI-driven workflow](../workspaces/cli-driven-workflow.md)), so a normal `terraform plan` or `apply` in a workflow can execute in Terrakube. Configure the `cloud` block as described there, then give the CLI a token:

```yaml
jobs:
  terraform:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - uses: hashicorp/setup-terraform@v3
        with:
          cli_config_credentials_hostname: terrakube-api.example.com
          cli_config_credentials_token: ${{ secrets.TERRAKUBE_PAT }}

      - run: terraform init
      - run: terraform plan
```

This keeps state, variables, policy checks and run history in Terrakube while the workflow stays in GitHub. For OpenTofu use `opentofu/setup-opentofu` the same way. If the registry has a different hostname from the API, add the token for both hostnames. See [CLI-driven Workflow](../workspaces/cli-driven-workflow.md).

### The Terrakube CLI

The [Terrakube CLI](../terrakube-cli/README.md) (`terrakube`) manages Terrakube itself and can trigger runs. Download a release binary ([releases](https://github.com/terrakube-io/terrakube-cli/releases)) and configure it with environment variables:

```yaml
jobs:
  run:
    runs-on: ubuntu-latest
    env:
      TERRAKUBE_API_URL: ${{ secrets.TERRAKUBE_ENDPOINT }}
      TERRAKUBE_TOKEN: ${{ secrets.TERRAKUBE_PAT }}
      TERRAKUBE_ORGANIZATION: my-organization
    steps:
      - name: Install the Terrakube CLI
        run: |
          VERSION=1.0.0-beta.11
          curl -sL "https://github.com/terrakube-io/terrakube-cli/releases/download/v${VERSION}/terrakube_${VERSION}_linux_amd64.tar.gz" | tar -xz
          sudo mv terrakube /usr/local/bin/

      - name: Start a plan
        run: terrakube job create --workspace my-workspace --command plan --output json
```

See [`job create`](../terrakube-cli/commands/job/create.md) and the other [commands](../terrakube-cli/commands/README.md).

### Other tools

Anything that can call HTTP can use the [API](../../api/getting-started.md) directly, and the [Terraform provider](https://registry.terraform.io/providers/terrakube-io/terrakube/latest/docs) manages Terrakube resources as code.
