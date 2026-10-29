# Executor

The executor runs the steps of a run: it clones your repository, runs Terraform or OpenTofu and your scripts, and reports back to the API.

* [Terraform Execution Flow](terraform-execution-flow.md): the working directory, extensions and the order commands run in.
* [Terraform Versions](terraform-versions.md): how a Terraform or OpenTofu version or constraint is resolved and cached.
* [Executor settings](../configuration/executor.md): every environment variable.
* [Self-Hosted Agents](../../../getting-started/deployment/self-hosted-agents.md) and [Ephemeral Agents](../../../getting-started/deployment/ephemeral-agents.md): deploying executors.
