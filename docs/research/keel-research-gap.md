# Keel Research Gap

Keel is not intended to be another general LLM benchmark. Its research gap is
the reliability of personal agents that act over time with memory, tools,
privacy boundaries, commitments, and recurring responsibilities.

## The Problem

Personal agents are moving from chat toward delegated work. They may remember
preferences, read files, call tools, summarize projects, draft messages, manage
recurring routines, and report progress later. This creates a different
evaluation problem than single-turn question answering.

An agent can produce a plausible final answer while still failing in ways that
damage trust:

- It remembered a stale preference and applied it confidently.
- It used the wrong integration or trusted unavailable data.
- It promised to do work later but never produced the artifact.
- It skipped an approval before a state-changing action.
- It leaked private context into the wrong channel.
- It retried blindly after partial failure and corrupted state.
- It completed the task but at unacceptable cost, latency, or tool count.

These are process failures. They require trace, state, and artifact evidence.

## What Existing Benchmarks Cover Well

The current benchmark landscape is strong in several areas:

- General assistant capability: GAIA and AgentBench.
- Tool and policy interaction: τ-bench, τ²-bench, and BFCL.
- Web, OS, and coding environments: WebArena, OSWorld, SWE-bench, Odysseys, and
  WildClawBench.
- Memory capability: LongMemEval, MemoryAgentBench, AMA-Bench, MemGym,
  MemoryArena, and LongMINT / MINTEval.
- Agent safety and security: Agent-SafetyBench, AgentHarm, OASB, and ATBench.
- Evaluation operations: OpenAI evals and trace grading, LangSmith, and Inspect
  AI.

Keel should build on this work rather than pretend the field is empty.

## What Remains Underserved

Existing benchmarks tend to be organized by one primary axis: web browsing,
function calling, coding, OS control, memory, safety, or general assistant
ability. Personal agents combine these axes.

The underserved evaluation target is the persistent operating partner:

- It knows user-specific context and must update it safely.
- It acts through tools and integrations that can fail.
- It must preserve privacy and source boundaries.
- It makes commitments across time.
- It handles recurring work, not only isolated tasks.
- It must expose enough evidence for users and maintainers to trust its behavior.

Keel's opportunity is to evaluate these coupled behaviors together.

## Keel's Research Thesis

Trustworthy personal agents require evidence across five layers:

1. **Outcome:** Did the task get solved or the artifact get produced?
2. **Trace:** Did the agent use memory, tools, retries, and handoffs correctly?
3. **State:** Did the environment, files, commitments, or integrations end in
   the right state?
4. **Boundaries:** Did the agent obey privacy, approval, source, and autonomy
   constraints?
5. **Efficiency:** Did it stay within acceptable cost, latency, token, and tool
   budgets?

Keel should make these layers inspectable, versioned, and comparable across
agent snapshots.

## Product Motivation

The broader product motivation points to the same gap. People do not want
personal agents only for isolated chat turns; they want persistent operating
partners with durable context, recurring responsibilities, visible commitments,
and trustworthy integrations.

The strongest observed failure modes were not weak chat answers. They were
broken trust loops: missing integrations, promised work that did not finish,
autonomy controls that did not map to scheduled jobs, and degraded briefs that
hid missing context.

Keel should translate these product lessons into public-safe, reproducible
evaluation cases that benefit the broader agent community.

## Design Consequences

Keel should prioritize:

- Synthetic but realistic personal-work scenarios.
- Trace schemas that capture tool calls, results, approvals, retries, memory
  events, commitments, artifacts, and failures.
- Scorers that combine deterministic checks, state checks, trace checks,
  rubric criteria, calibrated LLM judges, and human review.
- Reports that separate outcome, process, boundary, and efficiency scores.
- Regression suites for known trust failures.
- Privacy-first dataset rules: no raw personal data, no production transcripts,
  no unredacted traces.

## Non-Goals

Keel should not try to be:

- A universal base-model leaderboard.
- A replacement for WebArena, OSWorld, SWE-bench, τ-bench, or memory-specific
  benchmarks.
- A production telemetry store.
- A collection of private customer traces.
- A single scalar score for "agent quality."

Keel should instead become a public, research-aware measurement layer for
personal-agent trust.
