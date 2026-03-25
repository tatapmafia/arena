# Changelog

All notable changes to Arena are documented here.

Format based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/).
This project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [2.1.0] - 2026-03-25

### Added
- **Strategy system** — 3 debate flows: `adversarial` (default), `quick` (~5 min), `red-team` (1 vs N)
- **Judge agent** — separate evaluator that scores argument quality, evidence strength, intellectual honesty per agent. Auto-enabled for adversarial/red-team, `--no-judge` to disable
- **Argument Graph** — ASCII visualization in consensus.md showing attack/rebuttal structure. Language auto-detected from user's brief
- **Multi-model config** — `provider` + `model` per agent in debate.config.json. Schema ready for any LLM provider (Claude, OpenAI, etc.)
- **Human-in-the-loop** — Discovery Interview (Phase 0.5), assumption tagging (VERIFIED/UNVERIFIED), user checkpoints after each round with corrections mechanism
- **Calibrated critique** — Evidence basis (OBSERVED/INFERRED/SPECULATIVE) + confidence scoring on Fatal Flaws. Agents can honestly report "I couldn't find a real problem"
- **Intellectual Honesty rules** — no invented data, no made-up percentages, no fictional benchmarks

## [2.0.0] - 2026-03-25

### Added
- Initial open-source release
- **6-phase adversarial protocol** — Pitch → Critique → Rebuttal → Revised → Synthesis → Consensus
- **5 anti-groupthink mechanisms** — Mandatory Fatal Flaw, Pre-mortem, Devil's Advocate rotation, Disagreement scoring, Groupthink alarm
- **3 presets** — architecture, go-to-market, risk-assessment
- **Structured JSON output** — debate.schema.json + debate.result.json alongside markdown
- **Standalone mode** — works via Agent tool, no Agent Teams dependency
- **English-first** with language auto-detection from brief
- **Worked example** — open-source decision with full debate artifacts
- MIT License

[2.1.0]: https://github.com/tatapmafia/arena/compare/v2.0.0...v2.1.0
[2.0.0]: https://github.com/tatapmafia/arena/releases/tag/v2.0.0
