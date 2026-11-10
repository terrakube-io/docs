# Action Context

The context object contains data related to the Workspace that you can use inside the action. The properties depends on the [Action Type](action-types.md).

{% hint style="info" %}
Tip

Inside your action you can use console.log(context) to quickly explore the available properties for the context
{% endhint %}

## **context.workspace**

Contains the workspace information in the Terrakube api format. See the [docs](../../../../api/methods/workspace.md) for more details on the properties available.&#x20;

#### Examples

* **context.workspace.id**: Id of the workspace
* **context.workspace.attributes.name**: Name of the workspace
* **context.workspace.attributes.terraformVersion**: Terraform version

#### Available for action types

* workspace/action
* workspace/resourcedrawer/action
* workspace/resourcedrawer/tab

## **context.settings**

Contains the settings that you configured in the display criteria.

#### Examples

* **context.settings.Repository:** the value of repository that you set for the setting. Example:

<figure><img src="../../../../.gitbook/assets/image (475).png" alt=""><figcaption></figcaption></figure>

#### Available for action types

* workspace/action
* workspace/resourcedrawer/action
* workspace/resourcedrawer/tab

## **context.state**

For `workspace/action` this property contains the full terraform or open tofu state. For `workspace/resourcedrawer/action` and `workspace/resourcedrawer/tab` contains only the section of the resource

#### Available for action types

* workspace/action
* workspace/resourcedrawer/action
* workspace/resourcedrawer/tab

## **context.apiUrl**

Contains the Terrakube api Url. Useful if you want to use the [Action Proxy ](action-proxy.md)or execute a call to the Terrakube API.

#### Available for action types

* workspace/action
* workspace/resourcedrawer/action
* workspace/resourcedrawer/tab
