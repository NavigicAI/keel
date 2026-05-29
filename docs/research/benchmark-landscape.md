# Benchmark Landscape

Keel should learn from existing agent benchmarks without becoming a clone of any
single one. This landscape records the benchmark families most relevant to
personal-agent trust evaluation.

## Comparison Matrix

| Benchmark | Domain | Interaction Type | Memory | Tools | Safety / Privacy | Long Horizon | Grading Style | What Keel Should Learn |
|---|---|---|---|---|---|---|---|---|
| MemoryArena | Multi-session agentic tasks | Agent-environment sessions | Strong | Some | Limited | Strong | Task answers and memory-action success | Test whether memory changes later action. |
| MemGym | Memory across agent settings | Tool, research, coding, computer-use tasks | Strong | Strong | Limited | Strong | Memory-isolated scores | Separate memory failures from other failures where possible. |
| AMA-Bench | Agent trajectories | Agent-environment histories | Strong | Some | Limited | Strong | Causal trajectory questions | Ask why something happened, not only what happened. |
| MemoryAgentBench | Incremental memory | Multi-turn interaction | Strong | Limited | Limited | Medium | Capability-specific accuracy | Slice memory into retrieval, update, conflict, and forgetting skills. |
| LongMemEval | Long-term assistant memory | Chat histories | Strong | Limited | Some abstention | Medium | Reference answers | Include temporal reasoning, updates, and abstention. |
| LongMINT / MINTEval | Interference-heavy memory | Long contexts, dialogue, revisions | Strong | Limited | Limited | Strong | Recall and aggregation accuracy | Stress repeated updates and stale context. |
| Agent Memory Benchmark | Memory harness | Multiple memory datasets | Strong | Varies | Varies | Varies | Accuracy, latency, cost | Report operational metrics, not just score. |
| τ-bench | Customer-service domains | Tool-agent-user conversation | Limited | Strong | Policy compliance | Medium | State and policy checks | Evaluate user interaction, policy, and tool outcomes together. |
| τ²-bench | Dual-control telecom | Shared user-agent environment | Limited | Strong | Policy compliance | Medium | State and coordination checks | Model users who can also change environment state. |
| BFCL | Function calling | Static and multi-turn tool calls | Limited | Strong | Limited | Low / Medium | Tool-call correctness | Use deterministic schema and tool-argument checks. |
| ToolSandbox | Stateful tool use | Conversational tool workflows | Limited | Strong | Some | Medium | Milestone and state checks | Use hidden state and intermediate milestone grading. |
| WebArena | Web tasks | Browser environment | Limited | Strong | Some auth/state concerns | Medium | Environment state success | Verify resulting state, not claims. |
| VisualWebArena | Visual web tasks | Browser with visual observations | Limited | Strong | Some | Medium | Visual and state checks | Preserve screenshots or visual observations when relevant. |
| WorkArena | Enterprise web workflows | SaaS workplace tasks | Limited | Strong | Enterprise policy concerns | Medium | Workflow state checks | Model realistic SaaS work without losing Keel's personal-agent scope. |
| OSWorld | Desktop tasks | OS environment | Limited | Strong | Some system safety | Medium / Strong | Environment and task success | Use reproducible environments for computer-use evals. |
| SWE-bench | Software engineering | Repository issue fixing | Limited | Strong | Code safety indirectly | Medium | Test suites | Prefer artifact verification where possible. |
| GAIA | General assistant tasks | Web/tool-assisted questions | Limited | Medium | Limited | Medium | Reference answers | Borrow realism, avoid becoming generic assistant Q&A. |
| AgentBench | Broad interactive agents | Multiple environments | Limited | Strong | Varies | Medium | Environment-specific success | Multi-capability breadth needs coherent scope. |
| Odysseys | Long web tasks | Live web browsing | Limited | Strong | Some live-site concerns | Strong | Rubric and progress scoring | Score partial progress on realistic long tasks. |
| WildClawBench | Real-world CLI agent work | Tool-rich containers | Limited | Strong | Some | Strong | Tool traces and task success | Track duration, tool count, retries, and artifacts. |
| BrowseComp | Hard web research | Persistent browsing | Limited | Medium | Limited | Medium | Short-answer with evidence | Borrow evidence requirements, not obscure-fact focus. |
| TheAgentCompany | Simulated company | Cross-app professional tasks | Limited | Strong | Workplace policy concerns | Strong | Task and artifact checks | Include delegated work with collaborators and artifacts. |
| GDPval | Economic work products | Expert-created professional tasks | Limited | Varies | Domain dependent | Medium / Strong | Expert comparison and rubrics | Use expert-authored deliverable criteria when suites mature. |
| Vending-Bench | Simulated business | Long-term operations | Strong state | Medium | Limited | Strong | Long-run business outcome | Test drift and delayed consequences in recurring loops. |
| Agent-SafetyBench | Agent safety | Interactive safety cases | Limited | Strong | Strong | Medium | Safety-risk categories | Make safety a dedicated suite. |
| AgentHarm | Harmful agent requests | Agent misuse scenarios | Limited | Strong | Strong | Medium | Harmfulness checks | Include malicious user and unsafe tool-use cases. |
| OASB | Agent security controls | Security-control checks | Memory/context controls | Strong | Strong | Varies | Control maturity and scanner metrics | Translate security controls into regression checks. |
| ATBench | Trajectory safety | Long-horizon agent traces | Varies | Strong | Strong | Strong | Trajectory-level safety | Evaluate harm emergence across steps. |
| Agentic Benchmark Checklist | Benchmark meta-evaluation | Benchmark design review | N/A | N/A | Benchmark integrity | N/A | Checklist | Document success criteria, leakage, scoring, and limitations. |
| NIST / CAISI cheating guidance | Benchmark security | Agent evaluation abuse modes | N/A | Strong | Strong | N/A | Threat analysis | Design harnesses assuming tool-using agents may cheat. |

## What Keel Should Learn

- From memory benchmarks: personal memory must include updates, contradictions,
  abstention, consent, and action-coupled use.
- From tool-agent-user benchmarks: useful agents operate under policies and
  shared state, not just schemas.
- From long-horizon benchmarks: final success is often too coarse; partial
  progress, retries, tool count, and environment side effects matter.
- From coding and OS benchmarks: objective artifact verification is stronger
  than self-reported completion.
- From evaluation platforms: datasets, traces, scorers, runs, and reports should
  be distinct versioned artifacts.
- From safety benchmarks: safety/privacy suites should be independent release
  gates, not optional rubric notes.
- From benchmark-rigor work: public reports must disclose leakage controls,
  scoring limitations, failure modes, and anti-cheating assumptions.

## What Keel Should Avoid Copying

- Do not become a base-model leaderboard. Keel should evaluate model-harness
  configurations: prompts, tools, memory policy, runtime, approval gates, and
  retry behavior.
- Do not reward recall without testing whether the remembered fact was used
  appropriately.
- Do not rely on final-answer scoring when trace or state evidence is available.
- Do not treat LLM-as-judge scores as ground truth without calibration and bias
  checks.
- Do not build only domain-specific tasks that fail to generalize to personal
  agents across work, life, memory, tools, and recurring commitments.
- Do not include private transcripts or customer data for realism. Use synthetic,
  redacted, or explicitly approved cases.
- Do not publish benchmark tasks without a contamination and refresh strategy.

## Initial Keel Benchmark Families

1. **Memory continuity:** preferences, facts, corrections, temporal context, and
   abstention across sessions.
2. **Tool discipline:** correct tool selection, argument quality, budget limits,
   and avoiding unnecessary calls.
3. **Privacy boundaries:** consent-to-remember, sensitive data suppression,
   source boundaries, and unauthorized disclosure.
4. **Commitment tracking:** promises, due times, artifacts, verification, and
   honest completion/failure reporting.
5. **Integration failure recovery:** missing auth, stale data, failed reads,
   partial writes, retries, and user handoff.
6. **Recurring work:** daily/weekly briefs, project reviews, follow-up loops, and
   visible autonomy controls.
