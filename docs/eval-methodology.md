# Evaluation Methodology

Keel is designed to be trace-first, dataset-driven, and regression-oriented.
Final-answer scoring is necessary, but it is not enough for personal agents:
an answer can look acceptable while the agent misused memory, over-called
tools, leaked private context, skipped approval, or failed to recover cleanly.

Outcome evaluation asks whether the final result satisfied the user's intent.
Process evaluation asks whether the agent used memory, tools, delegation,
budgets, safety policies, and recovery behavior appropriately. Boundary
evaluation asks whether the agent respected privacy, permissions, approvals, and
source constraints. Efficiency evaluation asks whether the run stayed within
acceptable cost, latency, token, and tool budgets.

## Capability Areas

Design evaluation suites around personal-agent capabilities rather than generic
prompt categories.

- **Memory and continuity:** remembers explicit preferences, distinguishes user
  facts from task-local context, and avoids stale or invented memory.
- **Tool selection and sequencing:** chooses the right tools, avoids unnecessary
  calls, and orders actions coherently.
- **Long-running task completion:** handles multi-step work, intermediate
  artifacts, retries, and resumable progress.
- **Workspace behavior:** operates safely around files, calendars, email,
  browser state, shell commands, and app-specific APIs.
- **Recovery behavior:** detects tool failures, retries boundedly, reports
  blockers clearly, and avoids corrupting state after partial failure.
- **Privacy and safety:** respects approval boundaries, does not expose private
  data, and refuses unsafe or unauthorized actions.
- **Budget discipline:** tracks latency, cost, token use, and tool count as
  first-class quality signals.
- **Human handoff:** asks for approval or clarification only when necessary and
  preserves enough context for a human to continue.
- **Commitment tracking:** records promised work, due times, artifacts,
  verification state, and honest completion or failure notices.

## Dataset Strategy

Use versioned, replayable datasets. Start small, then broaden coverage.

- **Smoke suites:** 10-20 fast synthetic cases that validate harness plumbing and
  CI behavior.
- **Regression suites:** historical failures that must stay fixed.
- **Capability suites:** focused groups for memory, tools, recovery, privacy,
  and long-running work.
- **Adversarial suites:** prompt injection, unsafe tool requests, hidden
  instruction conflicts, sensitive data traps, and misleading tool outputs.
- **Golden suites:** representative cases reviewed by humans and used for model
  judge calibration.

Checked-in datasets must be synthetic, redacted, or explicitly approved for
publication. Raw personal data, production transcripts, and private examples
belong outside git unless they have gone through a redaction and publication
review.

## Trace Capture

Capture the full run, not only the final message. Trace grading is especially
important for agents because it reveals process failures that final-answer
grading misses.

Each run should capture:

- User, system, assistant, and developer-facing messages where safe to store.
- Tool calls, arguments, results, errors, and call IDs.
- Agent handoffs and subtask boundaries.
- Files or artifacts read, written, or returned.
- Cost, latency, token counts, and model/provider identifiers.
- Retry counts, backoff behavior, and recovery decisions.
- Final output and any structured result.

Trace records should be JSON-compatible and portable across local CLI runs, CI,
future dashboards, and external evaluation platforms.

## Evaluation Layers

Use multiple scorer families. No single scoring method is reliable enough for
agent quality.

1. **Deterministic checks:** exact requirements, schema validity, file
   existence, command success, approval state, tool budgets, and policy
   invariants.
2. **Trace checks:** tool sequencing, redundant actions, retries, handoffs,
   memory access, forbidden tool use, and recovery paths.
3. **State and artifact checks:** environment state, file outputs, generated
   reports, scheduled commitments, and integration side effects.
4. **Rubric scoring:** qualitative work products where exact answers are not
   stable.
5. **Pairwise comparison:** candidate run versus baseline run for quality,
   brevity, safety, latency, or cost.
6. **LLM-as-judge:** scalable rubric scoring after calibration against human
   labels.
7. **Human review:** new categories, ambiguous failures, calibration sets, and
   public benchmark design.
8. **Regression comparison:** previous accepted agent snapshots versus the
   candidate agent snapshot.

Keep final-answer scores separate from trace/process scores. A run can pass the
output rubric and still fail because it used a forbidden tool, exceeded a budget,
or skipped a required approval.

LLM-as-judge scores should be treated as calibrated measurements, not ground
truth. Prefer deterministic, state-based, or reference checks when the expected
behavior can be verified directly. When using judge models, record the judge
model, prompt version, calibration set, agreement evidence, and known bias
checks.

## Model-Harness Configuration

Report results at the model-harness configuration level. For agents, quality is
not only a property of the base model. It also depends on prompts, toolsets,
execution environment, memory policy, retry strategy, approval gates, and trace
handling.

Every run should identify:

- Agent or harness version.
- Model and provider.
- Prompt or instruction version.
- Toolset version.
- Dataset and suite version.
- Scorer version.
- Relevant runtime flags and budget settings.

## Regression Gates

The harness should produce reports, but releases need explicit gates.

- Smoke suite must pass in CI.
- Critical privacy and safety cases must not regress.
- Known historical failures must stay fixed.
- Candidate mean score must meet or exceed an agreed baseline threshold.
- Candidate cost, latency, and tool count must stay within configured budgets.
- Any new model-judge scorer must be calibrated on a human-reviewed golden set
  before it becomes a release gate.
- Public benchmark suites must document success criteria, contamination risks,
  anti-cheating assumptions, and known limitations.

Small suites should run on every pull request. Larger research suites can run on
schedule, before releases, or after major agent/harness changes.

## Public-Readiness Rules

Keel's public repository should remain publishable by default.

- Keep checked-in examples synthetic unless explicitly approved.
- Store sensitive reports and traces outside git.
- Avoid product secrets, raw production prompts, real user transcripts, and
  unredacted screenshots.
- Prefer scenario templates that can generate synthetic cases.
- Treat trace exports as sensitive by default.
- Review licensing, datasets, and docs before making the repository public.

## Core Loop

```text
scenario dataset
  -> agent adapter
  -> controlled run
  -> trace capture
  -> deterministic scorers
  -> rubric / LLM / human scorers
  -> run summary
  -> baseline comparison
  -> CI or release gate
```

## References

- Keel literature review:
  [docs/research/literature-review.md](research/literature-review.md)
- Keel benchmark landscape:
  [docs/research/benchmark-landscape.md](research/benchmark-landscape.md)
- Keel research gap:
  [docs/research/keel-research-gap.md](research/keel-research-gap.md)
- OpenAI evaluation best practices:
  https://platform.openai.com/docs/guides/evaluation-best-practices
- OpenAI trace grading:
  https://platform.openai.com/docs/guides/trace-grading
- OpenAI agent evals:
  https://platform.openai.com/docs/guides/agent-evals
- OpenAI Agents SDK tracing:
  https://openai.github.io/openai-agents-js/guides/tracing
- LangSmith evaluation concepts:
  https://docs.langchain.com/langsmith/evaluation-concepts
- LangSmith evaluation product overview:
  https://www.langchain.com/langsmith/evaluation
- Inspect AI evaluation guide:
  https://huggingface.co/docs/inference-providers/en/guides/evaluation-inspect-ai
- Harness-Bench:
  https://arxiv.org/abs/2605.27922
- Agent Harness Engineering survey:
  https://openreview.net/pdf/f358711a95aaaf61fdeffd4ef3fc60fba9b8da57.pdf
