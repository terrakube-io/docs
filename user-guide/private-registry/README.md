# Private Registry

Terraform provides a public registry, where you can share and reuse your terraform modules or providers, but sometimes you need to define a Terraform module that can be accessed only withing your organization.

Terrakube provides a private registry that works similarly to the [public Terraform Registry](https://developer.hashicorp.com/terraform/registry) and helps you share [Terraform providers](https://developer.hashicorp.com/terraform/language/providers) and [Terraform modules](https://developer.hashicorp.com/terraform/language/modules) privately across your organization. You can use any of the main [VCS providers](../vcs-providers/) to mantain your terraform module code.

{% hint style="info" %}
At the moment the Terrakube UI only support [publish private modules,](publishing-private-modules.md) but you can publish providers using the [Terrakube API](../../api/methods/provider.md)
{% endhint %}

In this section:

{% content-ref url="publishing-private-modules.md" %}
[publishing-private-modules.md](publishing-private-modules.md)
{% endcontent-ref %}

{% content-ref url="using-private-modules.md" %}
[using-private-modules.md](using-private-modules.md)
{% endcontent-ref %}
