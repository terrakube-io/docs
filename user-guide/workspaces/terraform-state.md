# Terraform State

Each Terrakube workspace has its own separate state data, used for jobs within that workspace. In Terrakube you can see the Terraform state in the standard [JSON format](terraform-state.md#json-state) provided by the Terraform cli or you can see a [Visual State](terraform-state.md#visual-state), this is a diagram created by Terrakube to represent the resources inside the JSON state.

### JSON State

Go to the workspace and click the **States** tab and then click in the specific state you want to see.

<figure><img src="../../.gitbook/assets/image (255).png" alt=""><figcaption></figcaption></figure>

You will see the state in JSON format

<figure><img src="../../.gitbook/assets/image (284).png" alt=""><figcaption></figcaption></figure>

You can click the **Download** button to get a copy of the JSON state file&#x20;

<figure><img src="../../.gitbook/assets/image (198).png" alt=""><figcaption></figcaption></figure>

### Visual State

Inside the **states** page, click the **diagram** tab and you will see the visual state for your terraform state.

<figure><img src="../../.gitbook/assets/image (270).png" alt=""><figcaption></figcaption></figure>

The visual state will also show some information if you click each element like the following:

<figure><img src="../../.gitbook/assets/image (368).png" alt=""><figcaption></figcaption></figure>

### Resources

All the resources can be shown in the overview page inside the workspace:

<figure><img src="../../.gitbook/assets/image (366).png" alt=""><figcaption></figcaption></figure>

If you click the resources you can check the information about each resouce like the folloing

<figure><img src="../../.gitbook/assets/image (367).png" alt=""><figcaption></figcaption></figure>
