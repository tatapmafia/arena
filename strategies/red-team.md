---
name: red-team
type: strategy
description: One defender vs N attackers. Stress-test a specific proposal or plan. The defender must survive all attacks.
min_agents: 3
max_agents: 6
judge: true
phases: [setup, discovery, defense, attack-rounds, rebuttal, synthesis, consensus]
---

# Red Team Strategy

One agent defends a proposal. All others attack it from different angles. The defender must respond to every attack. Tests whether an idea can survive organized opposition.

## Flow

```
Phase 0: Setup + Discovery Interview
Phase 1: Defense — one agent writes comprehensive defense of the proposal
Phase 2: Attack Rounds — each attacker writes one focused attack (SEQUENTIAL,
         not parallel — each reads previous attacks to avoid redundancy)
Phase 3: Rebuttal — defender responds to ALL attacks in one document
Phase 4: Judge evaluation (automatic)
Phase 5: Facilitator synthesis — survived attacks, fatal wounds, conditional risks
Phase 6: Consensus + Argument Graph + JSON output
```

## When to Use

- You have a SPECIFIC proposal/plan to stress-test
- The question is "should we do X?" not "X or Y?"
- You want to find every possible way X could fail
- Pre-mortem on a decision you're leaning toward

## Roles

- **Defender** (1): believes in the proposal, builds the strongest case
- **Attackers** (N-1): each attacks from a different angle

Recommended attacker angles:
- Financial risk
- Technical feasibility
- Market/competitive risk
- Operational complexity
- Opportunity cost (what are we NOT doing?)

## Phase 1: Defense

Launch defender agent:
```
You are defending this proposal: {proposal from brief}.
Write a comprehensive defense to {workspace}/defense.md.
Include: thesis, evidence, implementation plan, risk acknowledgment, pre-mortem.
Format: follow ./formats/pitch.md.
```

## Phase 2: Attack Rounds (SEQUENTIAL)

Launch each attacker ONE AT A TIME. Each reads previous attacks:
```
Read {workspace}/defense.md and all previous attacks in {workspace}/attacks/.
Write your attack to {workspace}/attacks/attack-{N}-{role}.md.

DO NOT repeat attacks already made by previous attackers.
Find a NEW angle of attack.

Format: follow ./formats/critique.md.
Fatal Flaw MANDATORY. Evidence basis MANDATORY.
```

Why sequential: prevents redundant attacks. Each attacker must find something NEW.

## Phase 3: Rebuttal

Launch defender:
```
Read ALL attack files in {workspace}/attacks/.
Write your rebuttal to {workspace}/rebuttal.md.
Respond to EVERY attack. Accept what you must. Defend what you can.
Format: follow ./formats/rebuttal.md.
```

## Phase 4: Judge

Judge reads defense + all attacks + rebuttal. Evaluates:
- Which attacks survived the rebuttal?
- Which were successfully defended?
- Overall: does the proposal survive red-teaming?

## Workspace Structure

```
{workspace}/
├── defense.md
├── attacks/
│   ├── attack-1-{role}.md
│   ├── attack-2-{role}.md
│   └── attack-N-{role}.md
├── rebuttal.md
├── judge-report.md
├── 04-synthesis.md
├── 05-final/consensus.md
└── debate.result.json
```

## Differences from Adversarial

| Aspect | Red Team | Adversarial |
|--------|----------|-------------|
| Structure | 1 vs N | Each pitches |
| Attacks | Sequential (no redundancy) | Parallel per round |
| Rounds | 1 defense + N attacks + 1 rebuttal | N full rounds |
| Focus | "Can X survive?" | "X or Y or Z?" |
| Best for | Go/no-go, pre-mortem | Multi-option decisions |
