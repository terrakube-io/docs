# Actions

## Terrakube Actions

Terrakube actions allow you to extend the UI to perform additional actions based on the OpenTofu/Terraform state and the Workspace information. This enables a dynamic state, enhancing integration with different tools directly from the infrastructure. Examples of what an action can do include:

* **Open Provider Documentation**: Add a button in the UI to navigate to the Terraform registry and view the documentation for a resource.
* **Display Data**: Add a panel in a tab to display attributes from the Terraform state.
* **Restart a VM**: Add a button to restart a virtual machine.
* **Open Resource in Azure**: Add a button to open the resource in Azure.

Terrakube provides several built-in actions that you can enable, some of which are already enabled by default. Below is a list of currently available built-in actions. If you are interested in contributing, please check the [Terrakube actions repository](https://github.com/AzBuilder/terrakube-actions).

### Available Built-in Actions

| ID                           | Name               | Category | Description                           |
| ---------------------------- | ------------------ | -------- | ------------------------------------- |
| terrakube.open-documentation | Open Documentation | General  | Open Terraform registry documentation |

### Creating an Action

1. Navigate to the settings in any Terrakube organization.
2. Click the "Add Action" button.
3. Provide the following information:

| Field                | Description                                                                                                    |
| -------------------- | -------------------------------------------------------------------------------------------------------------- |
| **Name**             | The name of the action. This will be displayed in the UI.                                                      |
| **Type**             | Defines the section where this action will appear. Check the docs for specific areas.                          |
| **Label**            | For tabs, this will be displayed as the tab name. For action buttons, it will be displayed as the button name. |
| **Category**         | Organizes the actions based on their function. Example: General, Azure, Cost, Monitoring.                      |
| **Version**          | The version of the action. Must follow semantic versioning (e.g., 1.0.0).                                      |
| **Description**      | A brief description of the action.                                                                             |
| **Display Criteria** | The conditions under which the action will appear. Define filters and settings for each filter.                |
| **Action**           | A JavaScript function equivalent to a React component. Receives the context with relevant data.                |
| **Active**           | A switch to enable or disable the action.                                                                      |

4. Click the "Save" button.

### Editing an Action

To edit an existing action:

1. Navigate to the settings in your Terrakube organization.
2. Locate the action you want to edit in the list and click the "Edit" button.
3. Modify the fields as needed.
4. Click the "Save" button to apply your changes.

### Disabling an Action

To disable an action:

1. Navigate to the settings in your Terrakube organization.
2. Locate the action you want to disable in the list and toggle the "Active" switch to the off position.
3. The action will no longer appear in the UI.
