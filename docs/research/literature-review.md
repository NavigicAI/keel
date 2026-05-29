# Keel Literature Review

Keel is grounded in a simple claim: personal-agent evaluation must measure the
work process, not only the final message. The relevant research spans agent
memory, tool-agent-user interaction, long-horizon environments, trace grading,
LLM-as-judge methodology, and agent safety.

This review is intentionally selective. It records the sources that should shape
Keel's benchmark design, scoring contracts, and public research posture.

## Summary

Current agent-evaluation work is converging on several lessons:

- Agents need trace or trajectory evaluation because many failures happen in
  tool selection, sequencing, recovery, policy adherence, and side effects.
- Memory evaluation is moving from recall benchmarks toward action-coupled,
  multi-session tasks where stale or incorrect memory can change behavior.
- Long-horizon web, OS, coding, and tool-use benchmarks show that realistic work
  requires stateful environments, partial-credit rubrics, reproducibility, and
  cost or efficiency reporting.
- LLM-as-judge can scale review, but it must be calibrated against human labels
  and supported by deterministic or state-based checks where possible.
- Safety, privacy, and security should be first-class suites for agents with
  tools, memory, files, credentials, networks, and delegated autonomy.

Keel's niche is personal-agent trust: evaluating whether an agent can preserve
context, use tools, respect boundaries, recover from failure, track commitments,
and complete recurring work over time.

## Agent Memory

### MemoryArena

- Source: <https://memoryarena.github.io/>
- Paper: <https://arxiv.org/abs/2602.16313>

MemoryArena is one of the closest precedents for Keel's memory thesis. It argues
that memory and action are often evaluated separately, then introduces
multi-session tasks where agents must learn from earlier actions and feedback,
distill useful memory, and use it in later subtasks.

Keel implication: memory cases should not only ask whether the agent can recall a
fact. They should test whether memory changes downstream decisions, tool use,
planning, and boundary handling.

### MemGym

- Source: <https://arxiv.org/abs/2605.20833>

MemGym frames memory as a long-horizon agent capability across tool-use dialogue,
deep research, coding, and computer-use settings. Its useful contribution for
Keel is the idea of isolating memory performance from other agent capabilities
where possible.

Keel implication: reports should separate memory failures from reasoning,
retrieval, tool, and environment failures when traces support that distinction.

### AMA-Bench

- Source: <https://ama-bench.github.io/>
- Paper: <https://arxiv.org/abs/2602.22769>

AMA-Bench focuses on memory for agentic applications with long trajectories and
causal structure. It is useful because personal agents need to remember why
something happened, not only that a string appeared earlier.

Keel implication: traces should support causal questions over commitments,
source access, user approvals, retries, and resulting artifacts.

### MemoryAgentBench

- Source: <https://github.com/HUST-AI-HYZ/MemoryAgentBench>
- Paper: <https://arxiv.org/abs/2507.05257>

MemoryAgentBench provides a useful capability grid for incremental multi-turn
memory: accurate retrieval, test-time learning, long-range understanding, and
selective forgetting or conflict resolution.

Keel implication: the first memory suites should be capability-sliced before
they become broad personal-work scenarios.

### LongMemEval

- Source: <https://arxiv.org/abs/2410.10813>

LongMemEval evaluates chat assistants on long-term interactive memory, including
information extraction, multi-session reasoning, temporal reasoning, knowledge
updates, and abstention.

Keel implication: personal-agent memory suites should include "do not know" and
"do not use this memory" cases, not only successful retrieval.

### LongMINT / MINTEval

- Source: <https://arxiv.org/abs/2605.18565>

LongMINT, also discussed as MINTEval, stresses memory under multi-target
interference: repeated updates, state tracking, multi-turn dialogue, revision
histories, and multi-target aggregation.

Keel implication: personal memory tests need contradiction, correction, and
stale-context scenarios because real assistants will receive changing user facts,
project states, and preferences.

### Agent Memory Benchmark

- Source: <https://agentmemorybenchmark.ai/>
- Code: <https://github.com/vectorize-io/agent-memory-benchmark>

Agent Memory Benchmark is useful as harness infrastructure rather than as a
single research paper. It aggregates memory datasets and reports practical
operational metrics such as latency, token use, retrieval calls, and scoring
logic.

Keel implication: memory reports should include accuracy alongside cost,
latency, retrieval behavior, abstention, and auditability.

### Memory Surveys

- Memory for Autonomous LLM Agents:
  <https://arxiv.org/abs/2603.07670>
- Memory in the Age of AI Agents:
  <https://arxiv.org/abs/2512.13564>

These surveys help stabilize terminology around memory formation, management,
retrieval, update, forgetting, factual memory, episodic memory, working memory,
and action-coupled memory.

Keel implication: docs should use precise memory terms and avoid collapsing all
memory behavior into retrieval.

## Tool-Agent-User Interaction

### τ-bench

- Source: <https://www.tau-bench.com/>
- Paper: <https://arxiv.org/abs/2406.12045>

τ-bench evaluates agents in realistic domains where they must converse with a
simulated user, call APIs, and follow domain policies. It is important because
agent reliability depends on tool use and policy adherence across multi-turn
interaction, not only final text.

Keel implication: personal-agent suites should include policy-like user
preferences, integration constraints, and state-changing tools with verifiable
outcomes.

### τ²-bench

- Paper: <https://arxiv.org/abs/2506.07982>

τ²-bench extends the tool-agent-user model into a dual-control environment where
both user and agent can act through tools in shared state.

Keel implication: personal-agent evaluation should eventually model users who
change state outside the agent, such as editing a calendar, reconnecting an
integration, or correcting a remembered preference.

### Berkeley Function Calling Leaderboard

- Source: <https://gorilla.cs.berkeley.edu/leaderboard>
- Paper: <https://openreview.net/forum?id=2GmDdhBdDk>

BFCL focuses on function-calling accuracy across tools, schemas, and providers.
It is narrower than Keel's scope but remains important for tool-selection and
argument-generation baselines.

Keel implication: Keel should use deterministic tool-call checks for low-level
tool correctness before asking broader process-quality questions.

### ToolSandbox

- Source: <https://arxiv.org/abs/2408.04682>

ToolSandbox evaluates stateful conversational tool use with dynamic checks over
intermediate and final milestones.

Keel implication: personal-agent tool suites should include hidden state,
milestones, and partial completion checks rather than only a final response.

## Long-Horizon Environments

### WebArena

- Paper summary: <https://huggingface.co/papers/2307.13854>
- Project: <https://webarena.dev/>

WebArena evaluates autonomous web agents in realistic websites with functional
state. It established a pattern of environment-grounded tasks where success can
be checked against a resulting state.

Keel implication: personal-agent tasks should verify resulting state or
artifacts, not only whether the final answer claims success.

### VisualWebArena

- Source: <https://arxiv.org/abs/2401.13649>

VisualWebArena extends web-agent evaluation to tasks that require visual
understanding of screenshots, layouts, and UI affordances.

Keel implication: if Keel evaluates browser or desktop agents, visual evidence
should be preserved in traces when it influences tool choices or user-visible
results.

### WorkArena

- Source: <https://arxiv.org/abs/2403.07718>
- WorkArena++: <https://arxiv.org/abs/2407.05291>

WorkArena evaluates enterprise browser workflows, especially ServiceNow-style
knowledge work. It is useful because many personal agents will operate across
SaaS surfaces rather than isolated tools.

Keel implication: Keel should learn from workplace workflow realism while
keeping its scope centered on personal-agent trust.

### OSWorld

- Source: <https://os-world.github.io/>
- Paper: <https://arxiv.org/abs/2404.07972>

OSWorld evaluates computer-use agents in real operating-system environments.
It highlights the difficulty of UI-grounded, multi-step work and the need for
reproducible environments.

Keel implication: if Keel later covers desktop or browser agents, environment
snapshots and reproducible state resets are essential.

### SWE-bench

- Source: <https://www.swebench.com/>
- Original paper: <https://arxiv.org/abs/2310.06770>
- SWE-bench Verified note:
  <https://openai.com/index/introducing-swe-bench-verified/>
- SWE-bench Live: <https://arxiv.org/abs/2505.23419>

SWE-bench evaluates coding agents on real GitHub issues and repository tests.
It is domain-specific, but it demonstrates the value of real artifacts and
objective verification through tests.

Keel implication: personal-work cases should include target artifacts and
verification steps whenever possible.

It is also a cautionary example: static public benchmarks can become less useful
as frontier systems overfit, train on, or route around known tasks. Keel should
track contamination and refresh strategy before becoming public.

### GAIA

- Paper: <https://arxiv.org/abs/2311.12983>

GAIA evaluates general AI assistants on real-world questions that often require
reasoning, web browsing, and tool use. It is closer to assistant capability than
to persistent personal-agent reliability.

Keel implication: Keel can borrow the emphasis on realistic multi-step tasks,
but should not become a general assistant Q&A benchmark.

### AgentBench

- Paper: <https://proceedings.iclr.cc/paper_files/paper/2024/hash/e9df36b21ff4ee211a8b71ee8b7e9f57-Abstract-Conference.html>

AgentBench evaluates LLMs as agents across multiple interactive environments.
It is useful as a broad early benchmark for agentic capability.

Keel implication: Keel should stay multi-capability, but organize around
personal-agent trust rather than environment variety for its own sake.

### Odysseys

- Paper: <https://arxiv.org/abs/2604.24964>
- Project: <https://odysseys-website.pages.dev/>

Odysseys evaluates long-horizon web agents on tasks derived from real browsing
sessions and uses rubric-style progress measures for realistic workflows.

Keel implication: long personal tasks should support partial-progress scoring
and should distinguish useful progress from final success.

### WildClawBench

- Paper: <https://arxiv.org/abs/2605.10912>
- Code: <https://github.com/InternLM/WildClawBench>

WildClawBench evaluates long-horizon real-world agent work inside reproducible
tool environments. It emphasizes actual tool use, wall-clock duration, and
multi-step execution.

Keel implication: Keel should track tool count, duration, failures, retries, and
artifact production as first-class metrics.

### BrowseComp

- Source: <https://arxiv.org/abs/2504.12516>
- Overview: <https://openai.com/index/browsecomp/>

BrowseComp evaluates persistent web browsing for hard-to-find facts.

Keel implication: evidence requirements and adversarial difficulty are useful,
but personal-agent evaluation should not collapse into obscure fact retrieval.

## Workplace and Professional Agents

### TheAgentCompany

- Source: <https://arxiv.org/abs/2412.14161>

TheAgentCompany simulates a software company with web, code, programs, and
coworker communication.

Keel implication: delegated work often has social and organizational context.
Keel should model handoffs, collaborator messages, and workplace artifacts where
those affect trust.

### GDPval

- Source: <https://arxiv.org/abs/2510.04374>
- Overview: <https://openai.com/index/gdpval/>

GDPval evaluates economically meaningful expert-created deliverables.

Keel implication: public Keel suites should eventually include work products
that subject-matter experts can judge, but initial suites should keep
deterministic and rubric criteria tractable.

### Vending-Bench

- Source: <https://arxiv.org/abs/2502.15840>

Vending-Bench evaluates long-term coherence in a simulated business.

Keel implication: recurring personal-agent loops should test drift, delayed
consequences, accounting-like state, and strategy consistency across time.

## Evaluation Methodology

### OpenAI Evaluation Best Practices, Agent Evals, and Trace Grading

- Best practices:
  <https://platform.openai.com/docs/guides/evaluation-best-practices>
- Agent evals: <https://platform.openai.com/docs/guides/agent-evals>
- Trace grading: <https://platform.openai.com/docs/guides/trace-grading>

These references provide practical guidance for production evaluation. The trace
grading docs are especially aligned with Keel: workflow-level agent errors
require structured scores over traces, not only final outputs.

Keel implication: the core data model should preserve examples, runs, traces,
scorers, reports, and release gates as separate artifacts.

### LangSmith Evaluation Concepts

- Source: <https://docs.langchain.com/langsmith/evaluation-concepts>
- Evaluation workflow: <https://docs.langchain.com/langsmith/evaluation>

LangSmith's documentation is useful for the production evaluation lifecycle:
datasets, examples, runs, traces, evaluators, experiments, online evaluation,
and conversion of production failures into regression datasets.

Keel implication: Keel should document an offline-to-online flywheel while
remaining provider-neutral.

### Inspect AI

- Source: <https://inspect.aisi.org.uk/>
- Hugging Face guide:
  <https://huggingface.co/docs/inference-providers/guides/evaluation-inspect-ai>

Inspect AI is an open-source evaluation framework from the UK AI Security
Institute. It is relevant because it treats tasks, solvers, scorers, sandboxes,
logs, and reproducibility as core concerns.

Keel implication: Keel should remain compatible with external eval systems and
avoid tying its contracts to a single provider dashboard.

### LLM-as-Judge and Rubric Calibration

- G-Eval: <https://arxiv.org/abs/2303.16634>
- Judging the Judges: <https://arxiv.org/abs/2406.12624>
- Position bias in LLM judges: <https://arxiv.org/abs/2406.07791>
- LLM-Rubric: <https://arxiv.org/abs/2501.00274>
- Autorubric: <https://arxiv.org/abs/2603.00077>

LLM-as-judge is useful for scale, but it is a noisy measurement layer. The
literature repeatedly points to calibration, bias checks, agreement metrics, and
careful rubric design.

Keel implication: LLM judges should be optional scorers with calibration
metadata, not the default source of truth.

## Safety, Privacy, and Security

### Agent-SafetyBench

- Paper: <https://arxiv.org/abs/2412.14470>
- Code: <https://github.com/thu-coai/Agent-SafetyBench>

Agent-SafetyBench evaluates safety failures in LLM agents rather than simple
chatbots. It is a useful reference for making safety an agent-workflow problem.

Keel implication: privacy and safety should have dedicated suites covering tool
misuse, data leakage, unsafe delegation, missing approvals, and policy failures.

### AgentHarm

- Paper: <https://arxiv.org/abs/2410.09024>
- Dataset: <https://huggingface.co/datasets/ai-safety-institute/AgentHarm>

AgentHarm measures harmfulness in LLM agents under malicious requests and
jailbreak attempts.

Keel implication: personal-agent evals need misuse cases for agents with tools,
files, memory, and connected accounts.

### OASB

- Source: <https://www.oasb.ai/>
- Controls: <https://www.oasb.ai/controls>

OASB frames agent security as controls and maturity levels, including identity,
authorization, credentials, supply chain, memory, monitoring, and isolation.

Keel implication: Keel's safety suites should include security-control checks
where agents touch credentials, tools, memory, browsers, or remote systems.

### ATBench

- Paper: <https://arxiv.org/abs/2604.02022>

ATBench focuses on trajectory-level long-horizon agent safety, where harms
emerge across multiple steps rather than in isolated prompts.

Keel implication: safety and privacy failures should be evaluated over traces and
state transitions, not only final refusals.

## Benchmark Rigor and Anti-Cheating

### Agentic Benchmark Checklist

- Source: <https://arxiv.org/abs/2507.02825>

The Agentic Benchmark Checklist documents recurring benchmark failure modes such
as weak tests, loose success criteria, exploitable scoring, noisy ground truth,
and incomplete reporting.

Keel implication: every public suite should document what it measures, what it
does not measure, how success is checked, and how leakage or contamination risks
are handled.

### NIST / CAISI on Cheating in Agent Evaluations

- Source: <https://www.nist.gov/caisi/cheating-ai-agent-evaluations>

NIST / CAISI warns that agents can exploit evaluation harnesses: looking up
answers, disabling tests, adding benchmark-specific logic, or abusing tool
access.

Keel implication: Keel should assume benchmarked agents can use tools to cheat.
Suites need sandbox controls, evidence capture, anti-leakage rules, and
transparent reporting.

## Keel Research Questions

The initial research program should focus on these questions:

1. Can an agent preserve and correctly apply personal context across sessions?
2. Can it update or suppress stale memory when user facts change?
3. Can it use tools without unnecessary, unsafe, or policy-violating actions?
4. Can it make commitments visible and avoid claiming unfinished work is done?
5. Can it recover from missing context, failed integrations, and partial
   execution without corrupting state?
6. Can it maintain acceptable cost, latency, tool count, and user-intervention
   burden?
7. Can it respect privacy boundaries, consent-to-remember boundaries, and
   authorization scopes while still being useful?
