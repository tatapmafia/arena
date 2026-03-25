---
name: adversarial
type: strategy
description: Full structured debate — each agent pitches, others attack with mandatory fatal flaws, speaker rebuts. The standard protocol.
min_agents: 3
max_agents: 7
judge: true
phases: [setup, discovery, initial, pitch-rounds, revised, synthesis, final, consensus]
---

# Adversarial Strategy (Default)

The full debate protocol. Each agent defends their position in sequential rounds while others attack with mandatory fatal flaws and devil's advocate rotation.

## Flow

```
Phase 0: Setup + Discovery Interview
Phase 0.6: Initial conclusions (parallel)
Phase 1: Pitch Rounds (sequential, one per agent)
  Per round: Pitch → Critique (parallel) → Rebuttal → Validation → User Checkpoint
Phase 1.5: Judge evaluation (automatic)
Phase 2: Revised conclusions (parallel)
Phase 3: Facilitator synthesis
Phase 4: Final conclusions (parallel)
Phase 5: Consensus + Argument Graph + JSON output
```

## When to Use

- High-stakes decisions (cost of being wrong is significant)
- Multiple genuinely different perspectives exist
- You need a thorough, auditable decision record
- Time available: ~20-40 minutes

## Pitch Round Structure

Each round has 5 steps:
1. **Pitch** — speaker writes defense with evidence, pre-mortem, assumptions
2. **Critique** — all other agents write critiques with mandatory Fatal Flaw (parallel)
3. **Rebuttal** — speaker responds to every critique
4. **Validation** — facilitator checks completeness + calibrated critique assessment
5. **User Checkpoint** — facilitator shows UNVERIFIED assumptions to user for correction

## Devil's Advocate

One critic per round is designated DA. Rotates so each agent serves exactly once. DA adds a "Devil's Advocate Attack" section — the most destructive possible argument, even if they personally agree.

## Judge

After all pitch rounds, a separate Judge agent reads ALL artifacts and evaluates argument quality, evidence strength, and logical consistency. Judge report feeds into synthesis.
