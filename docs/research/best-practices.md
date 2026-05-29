# Agent Evaluation Best Practices

Current agent-evaluation guidance points toward trace-first, versioned,
replayable harnesses rather than final-answer-only testing. Keel should use this
document as an operating checklist for benchmark and scorer design.

For the broader literature map, see:

- [Literature Review](literature-review.md)
- [Benchmark Landscape](benchmark-landscape.md)
- [Keel Research Gap](keel-research-gap.md)

## Useful Patterns

- Structure evals as datasets plus explicit scoring criteria.
- Use trace grading to evaluate the whole workflow, not only the final message.
- Capture tool calls, handoffs, approvals, budgets, retries, memory events,
  artifacts, and custom events.
- Prefer deterministic and state-based checks whenever the expected behavior can
  be verified without a judge model.
- Combine deterministic assertions, reference checks, trace checks, rubrics,
  pairwise comparisons, calibrated LLM judges, and human annotation.
- Compare agent snapshots across prompt, model, memory policy, toolset, runtime,
  approval, and harness changes.
- Gate small suites in CI and run broader suites on schedule or before releases.
- Report outcome, process, boundary, and efficiency metrics separately.

## Scoring Layers

Use the cheapest reliable evaluator for each claim:

1. **Deterministic checks:** exact text, schemas, tool calls, files, command
   results, state transitions, budgets, and policy invariants.
2. **Reference-answer checks:** semantic or exact comparison against expected
   outputs.
3. **Trace checks:** tool order, redundant actions, retries, memory access,
   approvals, handoffs, forbidden calls, and recovery paths.
4. **Rubric checks:** decomposed criteria for open-ended deliverables.
5. **Pairwise comparisons:** candidate run versus baseline run.
6. **LLM judges:** calibrated model-based scoring when deterministic checks are
   insufficient.
7. **Human review:** calibration sets, high-risk categories, ambiguous failures,
   and benchmark design review.

## LLM Judge Rules

- Treat LLM-as-judge as a noisy evaluator, not as ground truth.
- Calibrate against human-labeled examples before using judge scores as release
  gates.
- Prefer decomposed pass/fail or bounded criteria over one holistic score.
- Randomize order and run swap tests for pairwise judging.
- Report judge model, prompt version, calibration data, agreement, and known
  failure modes.
- Grade observable evidence in traces, state, and artifacts. Do not treat
  self-reported reasoning as proof.

## Safety and Privacy Rules

- Safety and privacy should be dedicated suites with release-blocking checks.
- Include prompt injection, data exfiltration, over-permissioned tools,
  credential exposure, unsafe tool use, unauthorized actions, memory leakage,
  unsafe delegation, missing audit logs, and skipped approvals.
- Include "should not remember," "should forget," and "should not use memory
  here" cases.
- Keep raw personal data, production transcripts, and unredacted traces out of
  git.

## Benchmark Integrity

- Document exactly what each suite measures and does not measure.
- Preserve provenance for synthetic, redacted, or approved examples.
- Avoid single scalar leaderboards as the primary truth signal.
- Track contamination risk and define a refresh strategy before public release.
- Assume tool-using agents may try to exploit the harness; sandbox where needed
  and verify observable outcomes.

## References

- OpenAI evaluation best practices: https://platform.openai.com/docs/guides/evaluation-best-practices
- OpenAI trace grading: https://platform.openai.com/docs/guides/trace-grading
- OpenAI agent evals: https://platform.openai.com/docs/guides/agent-evals
- OpenAI Agents SDK tracing: https://openai.github.io/openai-agents-js/guides/tracing
- LangSmith evaluation concepts: https://docs.langchain.com/langsmith/evaluation-concepts
- Inspect AI: https://inspect.aisi.org.uk/
- Agentic Benchmark Checklist: https://arxiv.org/abs/2507.02825
- NIST / CAISI cheating in AI agent evaluations: https://www.nist.gov/caisi/cheating-ai-agent-evaluations
