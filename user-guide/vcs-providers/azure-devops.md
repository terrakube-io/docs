# Azure DevOps

{% hint style="danger" %}
OAuth apps are deprecated in Azure DevOps.[https://devblogs.microsoft.com/devops/no-new-azure-devops-oauth-apps](https://devblogs.microsoft.com/devops/no-new-azure-devops-oauth-apps/) as an alternative for now a managed identity can be used but the setup is only available using the API, for more information please check:



&#x20;[https://github.com/terrakube-io/terrakube/pull/2101](https://github.com/terrakube-io/terrakube/pull/2101)
{% endhint %}

For using repositories from Azure Devops with Terrakube workspaces and modules you will need to follow these steps:

{% hint style="info" %}
**Manage VCS Providers** permission is required to perform this action, please check [team-management.md](../organizations/team-management.md "mention") for more info.
{% endhint %}

Navigate to the desired organization and click the **Settings** button, then on the left menu select **VCS Providers**&#x20;

<figure><img src="../../.gitbook/assets/image (385).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
If you prefer, you can add a new VCS Provider directly from the [Create workspace](../workspaces/creating-workspaces.md) or Create Module screen.
{% endhint %}

Click the **Azure Devops** button

<figure><img src="../../.gitbook/assets/image (197).png" alt=""><figcaption></figcaption></figure>

In the next screen click the link to [register a new OAuth Application](https://aex.dev.azure.com/app/register?mkt=en-US) in Azure Devops

<figure><img src="../../.gitbook/assets/image (268).png" alt=""><figcaption></figcaption></figure>

In the Azure Devops page, complete the required fields and click **Create application**

<table><thead><tr><th width="354">Field</th><th>Description</th></tr></thead><tbody><tr><td>Company Name </td><td>Your company name.</td></tr><tr><td>Application name</td><td>The name of your application or you can use the Organization name</td></tr><tr><td>Application website</td><td>Your application or website url </td></tr><tr><td>Callback URL</td><td>Copy the Callback URL from the Terrakube URL</td></tr><tr><td>Authorized scopes (checkboxes)</td><td>Only the following should be checked: Code (read) Code (status)</td></tr></tbody></table>

{% hint style="info" %}
You can complete the fields using the information suggested by terrakube in the VCS provider screen
{% endhint %}

<figure><img src="../../.gitbook/assets/image (266).png" alt=""><figcaption></figcaption></figure>

In the next screen, copy the **App ID** and **Client Secret**

<figure><img src="../../.gitbook/assets/image (327).png" alt=""><figcaption></figcaption></figure>

Go back to Terrakube to enter the information you copied from the previous step. Then, click the **Connect and Continue** button.

<figure><img src="../../.gitbook/assets/image (375).png" alt=""><figcaption></figcaption></figure>

You will see an Azure Devops window, click the **Accept** button to complete the connection

<figure><img src="../../.gitbook/assets/image (205).png" alt=""><figcaption></figcaption></figure>

Finally, if the connection was established successfully, you will be redirected to the VCS provider’s page in your organization. You should see the connection status with the date and the user that created the connection.

<figure><img src="../../.gitbook/assets/image (366).png" alt=""><figcaption></figcaption></figure>

And now, you will be able to use the connection in your workspaces and modules:

<figure><img src="../../.gitbook/assets/image (217).png" alt=""><figcaption></figcaption></figure>

### Webhooks

Azure DevOps (`dev.azure.com` and Azure DevOps Server) supports triggering workspace runs from repository activity, using either connection type (OAuth/Personal Access Token, or Service Principal/Managed Identity). There are two ways to detect changes:

**Inbound service hooks (default).** When a workspace with an Azure DevOps VCS is saved, Terrakube creates an Azure DevOps [service hook](https://learn.microsoft.com/azure/devops/service-hooks/overview) subscription that posts to the Terrakube webhook endpoint. Each delivery is authenticated with a per-webhook token, and commit status is reported back to Azure DevOps as the run progresses. Supported events:

| Terrakube event | Azure DevOps `eventType`                     |
| ---------------- | --------------------------------------------- |
| Push              | `git.push`                                    |
| Pull Request       | `git.pullrequest.created`, `git.pullrequest.updated` |
| Pull Request comment | `ms.vss-code.git-pullrequest-comment-event` |

Because service hooks are delivered from the Azure DevOps cloud, the Terrakube webhook endpoint must be reachable from the public internet, and the connection needs **Edit Subscriptions** permission to create them.

{% hint style="warning" %}
The [PR comment plan/apply workflow](../workspaces/webhooks.md#posting-plan-and-apply-results-on-pull-requests) (Post Plan on PR / Allow Apply via PR Comment, `terrakube plan` / `terrakube apply` comments) is currently only available for GitHub, GitLab, and Bitbucket. Azure DevOps pull request events can trigger jobs, but results aren't posted back as PR comments yet.
{% endhint %}

**Outbound commit polling (opt-in).** If Terrakube runs on a private network that the Azure DevOps cloud can't reach, inbound service hooks will silently fail. Polling reverses the direction: Terrakube periodically makes an outbound call to fetch the tip commit of each Azure DevOps workspace branch and starts a run when it changes, reusing the same credentials already used for cloning — no inbound network exposure required. Polling is disabled by default; enable it with these API environment variables:

| Environment variable | Default | Description |
| --------------------- | ------- | ------------ |
| `AzureDevOpsPollingEnabled` | `false` | Enable outbound commit polling. |
| `AzureDevOpsPollingInterval` | `15000` | Milliseconds between polls of each workspace branch. |
| `AzureDevOpsPollingInitialDelay` | `10000` | Milliseconds to wait after startup before the first poll. |

The first observed commit for a branch only records a baseline and does not trigger a run. Branch and file-path filters on the workspace webhook are applied the same way as for inbound pushes.
