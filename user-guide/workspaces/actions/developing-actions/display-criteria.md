# Display Criteria

Display Criteria offer a flexible way to determine when an action will appear. While you can add logic inside your component to show or hide the action, using display criteria provides additional advantages:

* **Optimize Rendering:** Save on the loading cost of each component.
* **Multiple Criteria:** Add multiple display criteria based on your requirements.
* **Conditional Settings:** Define settings to be used when the criteria are met.

A display criteria is a JavaScript condition where you can use the data from the [Action Contex](action-context.md)t to decide if the action will appear.

### Examples

#### Always display the action

```javascript
true
```

#### Display the action for Workspaces with specific Terraform version.

```javascript
context.workspace.attributes.terraformVersion === "1.0.0"
```

#### Display the action for Azure resources

```javascript
context.state.provider.includes("azurerm")
```

#### Display the action only for Azure VM

```javascript
context.state.type.includes("azurerm_virtual_machine")
```

### Display Criteria Settings

You can set specific settings for each display criteria, which is useful when the action provides configurable options. For example, suppose you have an action that queries GitHub issues using the GitHub API and you want to display the issues using GraphQL. Your issues are stored in two different repositories: one for production and one for non-production.

In this case, you can create settings based on the display criteria to select the repository name. This approach allows your action code to be reusable. You only need to change the settings based on the criteria, rather than modifying the action code itself.

For instance:

* **Production Environment:** For workspaces starting with prod, use the repository name for production issues.

```javascript
context.workspace.attributes.startsWith("prod")
```

* **Non-Production Environment:** Use the repository name for non-production issues

```javascript
context.workspace.attributes.startsWith("dev")
```

By setting these configurations, you ensure that your action dynamically adapts to different environments or conditions, enhancing reusability and maintainability.

<figure><img src="../../../../.gitbook/assets/image (28).png" alt=""><figcaption><p>Multiple conditions using settings</p></figcaption></figure>

### Sensitive Settings

You might be tempted to store secrets inside settings; however, display criteria settings don't provide a secure way to store sensitive data. For cases where you need to use different credentials for an action based on your workspace, organization or any other condition, you should use template notation instead. This approach allows you to use[ Workspace Variables](../../../terrakube-cli/commands/terrakube-workspace/workspace-variable/) or [Global Variables](../../../organizations/global-variables.md) to store sensitive settings securely.

For example, instead of directly storing an API key in the settings, you can reference a variable:

* For the development environment: `${{var.dev_api_key}}`
* For the production environment: `${{var.prod_api_key}}`

<figure><img src="../../../../.gitbook/assets/image (29).png" alt=""><figcaption></figcaption></figure>

For the previous example, you will need to create the variables at workspace level or use global variables with the names `dev_api_key`and `prod_api_key`

For more details about using settings in your action and template variables check [Action Proxy](action-proxy.md).

### Multiple Display Actions

You can define multiple display criteria for your actions. In this case, the first criteria to be met will be applied, and the corresponding settings will be provided via the context.&#x20;

