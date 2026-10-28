# Executor Log Output

What the executor does with the output of a step (the lines Terraform, OpenTofu and scripts print), and the settings that control it. The defaults suit most installations.

{% hint style="info" %}
Available from Terrakube 2.34.0.
{% endhint %}

### Where output goes

* **Saved output**: every line, whole, written to storage when the step ends. This is what the **Console** tab and the download show after the step.
* **Live stream**: what viewers read while the step runs, through Redis (or the API when `io.executor.log-via-api=true`). It is bounded and may fall behind, but never slows the run.
* **Executor service log** (`kubectl logs`): **off by default since 2.34.0**. Output was previously copied here too, which doubled log ingestion cost and exposed printed values to anyone who can read cluster logs. Set `ExecutorLogsEchoToServiceLog=true` to restore it.

### Output survives executor failure

While a step runs, the executor also copies its output to storage in numbered parts (`tfoutput/<org>/<job>/<step>.parts/`). They are deleted when the final output is saved. If the executor dies first, the API serves the parts joined in order and the Console shows a warning that the output is incomplete. If the API is unreachable at the end of a plan or apply, the final structured result is spooled to storage and applied once the API returns, without re-running Terraform.

### Settings

Environment variables on the executor unless noted.

| Variable | Default | Meaning |
| --- | --- | --- |
| `ExecutorLogsEchoToServiceLog` | `false` | Also write output to the executor's own log. |
| `ExecutorLogsLiveLineMaxBytes` | `65536` | Longest line sent to the live stream; longer lines are cut there only. |
| `ExecutorLogsStepOutputMemoryBytes` | `4194304` | Output kept in memory per step before moving to a temporary file. A step printing more than this needs scratch space. |
| `ExecutorLogsArchiveEnabled` | `true` | Copy output to storage in parts while the step runs. |
| `ExecutorLogsArchivePartBytes` | `1048576` | Write a part once this many new bytes exist. |
| `ExecutorLogsArchiveIntervalSeconds` | `30` | Also write a part after this long if there is anything new. |
| `ExecutorLogsArchiveMaxParts` | `1000` | Most parts per step. |
| `ExecutorLogsRedisMaxLen` | `200000` | Entries kept in a job's live stream. |
| `ExecutorLogsApiQueueMaxBytes` | `16777216` | With `log-via-api`, bytes queued while the API is slow; oldest lines are dropped from the live view first. |
| `ExecutorStructuredOutputQueueMaxBytes` | `67108864` | Memory budget for structured-output snapshots waiting to be saved. |
| `LogsAppendMaxBytes` (API) | `4194304` | Largest log request the API accepts. |

Every limit affects only the live view. The run and the saved output are never trimmed.

### Run limits

Runs that stay open too long are closed by Terrakube. Set these on the API:

| Variable | Default | Applies to |
| --- | --- | --- |
| `InactiveJobsMaxAgeHours` | `6` | Pending, approved, queued and running runs are closed as failed. Time spent waiting for approval or for an earlier run does not count. |
| `InactiveJobsWaitingApprovalTimeoutHours` | `0` (off) | Runs waiting for approval are cancelled after this many hours. |
