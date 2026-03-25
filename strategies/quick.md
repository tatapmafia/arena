---
name: quick
type: strategy
description: Fast cross-critique — no pitch rounds. Each agent analyzes, then critiques others in parallel. ~5 minutes.
min_agents: 3
max_agents: 5
judge: false
phases: [setup, discovery, initial, cross-critique, synthesis, consensus]
---

# Quick Strategy (Easy Mode)

Skip pitch rounds entirely. Each agent writes an initial analysis, reads others, writes critiques, facilitator synthesizes. Done in ~5 minutes.

## Flow

```
Phase 0: Setup + Discovery Interview (brief)
Phase 1: Initial analysis (all agents parallel)
Phase 2: Cross-critique (all agents parallel — each critiques all others)
Phase 3: Facilitator synthesis + Argument Graph
Phase 4: Consensus document + JSON output
```

## When to Use

- Lower-stakes decisions ("should we use library X or Y?")
- Time-sensitive — need an answer in 5-10 minutes
- You want structured disagreement but don't need full adversarial rounds
- Exploring a question before committing to a full debate

## Phase 1: Initial Analysis

Launch all agents in parallel:
```
Read 00-brief.md. Write your analysis to 01-initial/{role}.md.
Include: thesis, evidence, confidence (1-10), assumptions (VERIFIED/UNVERIFIED).
```

## Phase 2: Cross-Critique

After all initial analyses are written, launch all agents in parallel:
```
Read ALL files in 01-initial/. For each OTHER agent, write a critique
to 02-critiques/{your_role}-on-{their_role}.md.

Format: follow ./formats/critique.md.
Fatal Flaw MANDATORY. Evidence basis (OBSERVED/INFERRED/SPECULATIVE) MANDATORY.
If you cannot find a genuine flaw — mark as SPECULATIVE with low confidence.
```

## Phase 3: Synthesis

Facilitator reads all initial analyses + all critiques. Writes:
- `04-synthesis.md` — consensus zones, contradictions, blind spots
- User checkpoint: show UNVERIFIED assumptions for correction

## Phase 4: Consensus

Facilitator writes `05-final/consensus.md` + `debate.result.json` + Argument Graph.
No revised conclusions or final conclusions — quick mode skips these.

## Differences from Adversarial

| Aspect | Quick | Adversarial |
|--------|-------|-------------|
| Pitch rounds | None | One per agent |
| Rebuttals | None | After each pitch |
| Revised conclusions | None | After all rounds |
| Final conclusions | None | After synthesis |
| Judge | Off by default | On by default |
| User checkpoints | 1 (after cross-critique) | After each round |
| Time | ~5 min | ~20-40 min |
| Best for | Quick decisions, exploration | High-stakes, auditable |
