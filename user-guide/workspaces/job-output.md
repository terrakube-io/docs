# Job Output

Each plan and apply step in a job's output has a **Structured** / **Console** toggle. **Console** shows the raw Terraform/OpenTofu CLI output; **Structured** renders the same plan or apply as a resource-by-resource change list.

<figure><img src="../../.gitbook/assets/job-output-structured.png" alt=""><figcaption><p>A plan step with the Structured view selected</p></figcaption></figure>

### Structured view

Each resource in the structured view shows:

* An **action** badge — `create`, `update`, `delete`, `replace` (destroy + create), `import`, or `no-op`
* The specific attributes changing, with unchanged attributes collapsed by default (expanded by default for imports, since the point of reviewing an import is usually to check what already matches)
* A provider icon next to the resource address

Use the **filter resources by address** search box and the **operation filter** (multi-select, with a count per operation) to narrow a large plan down to the changes you care about.

{% hint style="info" %}
Sensitive values are redacted before they leave the executor, in both Structured and Console views — not just hidden client-side.
{% endhint %}

### Apply-time structured output

During an apply, the structured view updates live, per resource, as Terraform/OpenTofu applies each change — you don't have to wait for the whole apply to finish to see which resources have completed. Once the apply finishes, an **Outputs** section lists the workspace's Terraform outputs, with sensitive outputs redacted the same way as resource attributes.

### Diagnostics and output changes

* A **Diagnostics** panel above the steps lists every error and warning for the run, independent of the resource filters, with the resource address and source location where known.
* When a plan changes only root module outputs and no resources, an **Output changes** panel shows the diff instead of "no changes".
* Each plan, apply and destroy step says whether its result is final ("Final results") or still being recovered. If the executor loses contact with the API, the final output is saved to cloud storage and applied automatically once the API is back; the run does not need to be repeated. If a step ends without its saved output, the Console shows a warning and serves the output archived so far.
* The live log is bounded so that very noisy providers cannot exhaust memory: lines over 64 KiB are shortened in the live view only, and the Console after the step and the download always contain the full text.

Operators can tune these limits and the archiving of in-progress output; see [Executor log output](../../getting-started/deployment/executor-log-output.md).
