# Keel

AI agents are moving from answering questions to carrying real personal work.
Keel exists to help the community measure when those agents are reliable enough
to trust.

A ship's keel gives it stability and direction; Keel does the same for AI agents
before they carry human work.

Keel is an early public research project from NavigicAI. This repository is the
curated public home for reviewed research notes, benchmark maps, methodology,
and public-safe examples for personal-agent evaluation.

## Why Keel Exists

Final-answer benchmarks miss many of the failures that matter for personal
agents. An answer can look good while the agent used stale memory, called the
wrong tool, skipped an approval, leaked sensitive context, or failed to recover
after a partial error.

As agents begin to act on behalf of people, reliability becomes a process
question as much as an output question. Teams need to measure memory, tool use,
privacy, recovery, cost, latency, and long-running task completion before
trusting agents with real personal work.

Keel is our contribution to that measurement layer: public-safe scenarios,
trace-first scoring, reusable contracts, and regression-oriented workflows that
help teams compare and improve personal agents rigorously.

## What Keel Evaluates

Keel focuses on personal-agent behavior that affects trust:

- Memory continuity across turns, sessions, and tasks.
- Tool selection, sequencing, and budget discipline.
- Delegated execution over longer workflows.
- Recovery from partial failure or missing context.
- Privacy, approval, and policy-boundary handling.
- Commitment tracking and honest completion reporting.
- Final outcome quality, latency, and cost.

## Research Foundation

Keel builds on recent work in trace grading, tool-agent-user benchmarks,
long-horizon web and computer-use benchmarks, agent memory evaluation,
LLM-as-judge calibration, and agent safety.

- [Literature Review](docs/research/literature-review.md) summarizes the sources
  that shape Keel's design.
- [Benchmark Landscape](docs/research/benchmark-landscape.md) compares adjacent
  benchmarks and what Keel should learn from them.
- [Keel Research Gap](docs/research/keel-research-gap.md) explains why
  personal-agent trust needs its own evaluation layer.
- [Best Practices](docs/research/best-practices.md) records benchmark and scorer
  design rules for contributors.
- [Evaluation Methodology](docs/eval-methodology.md) describes Keel's trace-first
  approach.

## Status

Keel is early. This public repository currently contains research and
methodology docs. Public-safe datasets, runnable examples, and reusable tooling
will be added after review.

## Scope Boundaries

Keel is not:

- A production telemetry store.
- A place for raw customer transcripts or private traces.
- A benchmark leaderboard for base models alone.
- A single scalar score for agent quality.

## Public Data Rules

Checked-in datasets must be synthetic, redacted, or explicitly approved for
publication. Do not contribute raw personal data, production transcripts,
customer documents, private prompts, secrets, or unredacted traces.

## License

Keel is published under the Apache License 2.0. See [LICENSE](LICENSE).

## Citation

If Keel informs your research or evaluation work, please cite the repository
metadata in [CITATION.cff](CITATION.cff).
