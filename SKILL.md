---
name: debate
description: "Structured adversarial debate between AI agents. Forces disagreement, mandatory fatal flaws, devil's advocate rotation. Produces decision-quality consensus documents. Use when 3+ experts need to stress-test a decision."
version: 2.1.0
---

# debate-protocol

Structured adversarial debate between 3-5 AI agents. Each defends their position, others attack with mandatory fatal flaws, facilitator synthesizes into actionable consensus.

**Category:** Multi-Agent Adversarial Reasoning — not cooperation, productive conflict.

## When to Use

- High-stakes decision where groupthink is dangerous
- Need to stress-test assumptions before committing
- Want structured disagreement, not one confident answer
- 3+ genuinely different perspectives exist on the problem

**NOT for:** code review, sequential tasks, questions with clear right answers.

## Quick Start

```
/debate "Should we rewrite our auth system?"
/debate --quick "Redis or Memcached?"
/debate --preset architecture "Monolith or microservices?"
/debate --strategy red-team "We should migrate to Kubernetes"
/debate --no-judge "Low-stakes question"
```

## Strategies

| Strategy | Flow | Time | Best for |
|----------|------|------|----------|
| `adversarial` (default) | Pitch rounds → Judge → Revised → Synthesis → Consensus | ~20-40 min | High-stakes, multi-option decisions |
| `quick` | Initial analysis → Cross-critique → Synthesis → Consensus | ~5 min | Quick decisions, exploration |
| `red-team` | Defense → Sequential attacks → Rebuttal → Judge → Consensus | ~15-25 min | Stress-test a specific proposal |

Strategy files: `./strategies/`. Custom strategies: drop a `.md` file there.

## Presets

| Preset | Agents | Best for |
|--------|--------|----------|
| `architecture` | Pragmatist, Purist, Operator | Build vs buy, tech selection, migration |
| `go-to-market` | Growth, Product, Finance, Customer | Launch strategy, pricing, channels |
| `risk-assessment` | Optimist, Pessimist, Realist | Investment, go/no-go, any high-stakes bet |

Custom presets: drop a `.md` file in `presets/`. See existing presets for format.

## Multi-Model Support

Optionally specify `provider` and `model` per agent in `debate.config.json`:
```json
{"role": "optimist", "provider": "claude", "model": "opus"}
{"role": "pessimist", "provider": "openai", "model": "gpt-4o"}
```
Currently only Claude is supported via Agent tool. Config is ready for any provider — adapters can be added later.

---

## Execution Flow

### Strategy Loading

Read the strategy file from `./strategies/{strategy}.md` (default: `adversarial`).
The strategy defines: phases, min/max agents, whether judge is enabled.
Follow the strategy's phase sequence — do NOT hardcode phases.

For `--quick`: load `./strategies/quick.md` and follow its simplified flow.
For `--strategy red-team`: load `./strategies/red-team.md`.

### Phase 0: Setup

**0.1 Determine participants**

If preset specified: load agents from `./presets/{preset}.md`.
Otherwise ask user for:
- Role names (short identifiers: strategist, finance, ops)
- Role descriptions (1-2 sentences each — expertise + key question they ask)
- Topic / question to debate

**0.2 Create workspace**

```bash
WORKSPACE="/tmp/debate-$(echo '{topic}' | tr ' ' '-' | tr '[:upper:]' '[:lower:]' | head -c 30)"
mkdir -p "$WORKSPACE"/{01-initial,02-rounds,03-revised,05-final}
```

**0.3 Write debate.config.json**

```json
{
  "id": "debate-{topic-slug}-{date}",
  "question": "{the question being debated}",
  "strategy": "adversarial",
  "preset": "{preset name or null}",
  "perspectives": [
    {"role": "{role}", "brief": "{description}"}
  ],
  "constraints": {
    "max_rounds": "{N agents}",
    "output_format": "markdown+json"
  }
}
```

**0.4 Write 00-brief.md**

Problem statement, context, constraints. This is shared context for all agents. Include relevant data, links, background.

**0.5 Discovery Interview (MANDATORY)**

Before launching agents, interview the user. Agents cannot ask the user questions — only YOU can. Get the context they'll need:

```
1. "What context do the agents NEED to know that isn't obvious from the question?"
   (industry specifics, team size, budget, past attempts, constraints)

2. "What assumptions should agents NOT make?"
   (e.g., "we can't hire — budget is frozen", "we already tried X and it failed")

3. "What hard data do you have that agents should use instead of guessing?"
   (actual numbers, metrics, timelines, costs)
```

Append answers to `00-brief.md` under a `## User Context` section. This grounds the debate in reality.

**0.6 Collect initial conclusions**

Launch all agents in parallel using the Agent tool (with `run_in_background: true`):

Each agent prompt (from `./templates/debater-prompt.md`):
- Full role description and perspective
- Anti-groupthink rules
- Contents of 00-brief.md
- Task: "Read 00-brief.md. Write your initial analysis to {workspace}/01-initial/{role}.md. Include your thesis, key evidence, and initial confidence (1-10)."

Wait for all agents to complete. Read their initial conclusions.

**0.7 Define pitch order and devil's advocate rotation**

Default order: as listed in config. For DA rotation: assign the agent whose initial conclusion MOST contradicts the pitcher. Each agent serves as DA exactly once.

---

### Phase 1: Pitch Rounds

One round per agent, sequential. Each round has 4 steps:

#### Step 1: Pitch

Launch pitcher agent (Agent tool):
```
Pitch Round {N}. You are defending your position.

Read:
- {workspace}/00-brief.md (context)
- {workspace}/01-initial/{role}.md (your initial conclusion)
- ALL files in {workspace}/01-initial/ (other agents' positions)

Write your defense to: {workspace}/02-rounds/round-{N}-{role}/pitch.md

Format: follow ./formats/pitch.md EXACTLY.
Pre-mortem and Key Assumptions are MANDATORY.
```

Wait for completion.

#### Step 2: Critique (all other agents in parallel)

Launch each other agent (Agent tool, `run_in_background: true`):
```
Critique Round {N}. {pitcher_role} is defending their position.

Read:
- {workspace}/02-rounds/round-{N}-{role}/pitch.md
- {workspace}/01-initial/{pitcher_role}.md (author's full context)

Write critique to: {workspace}/02-rounds/round-{N}-{role}/critique-{your_role}.md

Format: follow ./formats/critique.md EXACTLY.
Fatal Flaw is MANDATORY. "I generally agree" = FAILURE.
```

For the devil's advocate in this round, ADD:
```
You are the DEVIL'S ADVOCATE this round. Add a "Devil's Advocate Attack" section.
Find the MOST DESTRUCTIVE argument against the author's position.
Even if you personally agree — attack as hard as possible.
Historical examples, industry data, worst-case scenarios.
```

Wait for all critics to complete.

#### Step 3: Rebuttal

Launch pitcher agent:
```
Rebuttal Round {N}. Respond to your critics.

Read ALL critique-*.md files in: {workspace}/02-rounds/round-{N}-{role}/

Write response to: {workspace}/02-rounds/round-{N}-{role}/rebuttal.md

Format: follow ./formats/rebuttal.md EXACTLY.
Respond to EVERY critique — do not skip any.
```

Wait for completion.

#### Step 4: Lead Validation

Read the rebuttal. Verify:
- [ ] Author responded to each critic's Fatal Flaw
- [ ] Responses are substantive (not dismissals)
- [ ] Updated Thesis is present

If incomplete: re-launch the agent with specific instruction to address the gap.

**Calibrated critique check:** If ALL critics marked their Fatal Flaw as SPECULATIVE with confidence < 5 → the pitcher's position is strong. Note this in synthesis.

#### Step 5: User Checkpoint (after each round)

Collect all UNVERIFIED assumptions from this round's pitch + critiques. Present to the user:

```
Agents made these assumptions in Round {N}:
- [UNVERIFIED] "{assumption 1}" (from {role})
- [UNVERIFIED] "{assumption 2}" (from {role})

Which are correct? Which are wrong? Anything to add?
```

Write user's corrections to `{workspace}/corrections/round-{N}.md`:
```markdown
# User Corrections — Round {N}
## Confirmed
- {assumption} — confirmed
## Corrected
- {assumption} — WRONG. Actual: {user's correction}
## New Context
- {anything user wants to add}
```

Instruct agents in the NEXT round: "Read {workspace}/corrections/round-{N}.md before writing."

**Before each round:** `mkdir -p "$WORKSPACE/02-rounds/round-{N}-{role}" "$WORKSPACE/corrections"`

---

### Phase 1.5: Judge Evaluation (if strategy.judge = true)

Unless `--no-judge` was specified, launch a separate Judge agent after all pitch rounds:

```
You are the JUDGE. You did NOT participate in the debate. You OBSERVE and ASSESS.

Read ALL artifacts in {workspace}:
- All pitches, critiques, rebuttals in 02-rounds/
- All corrections in corrections/

Write your evaluation to: {workspace}/judge-report.md

Format: follow ./formats/judge-report.md EXACTLY.
Evaluate: argument quality, evidence strength, intellectual honesty per agent.
Identify: strongest arguments that survived, weakest that collapsed.
Score: overall debate quality 1-10.
```

The facilitator uses the judge report in Phase 3 (Synthesis).

---

### Phase 2: Revised Conclusions

After ALL pitch rounds (and judge evaluation) complete. Launch all agents in parallel:

```
The debate rounds are complete. Time to revise your position.

Read ALL rounds in {workspace}/02-rounds/:
- Every pitch.md, every critique-*.md, every rebuttal.md

Write your revised conclusion to: {workspace}/03-revised/{your_role}.md

Format: follow ./formats/revised.md EXACTLY.
Agreement Map is MANDATORY — who you agree with, who you disagree with, and why.
Confidence trajectory is MANDATORY — how your confidence changed through the debate.
```

Wait for all agents.

**Groupthink check:** Read all revised conclusions. If Agreement Maps show universal agreement on all points — this is a RED FLAG. Re-launch the agent who had the strongest original disagreement:

"All agents now agree on everything. Is this genuine consensus or group pressure? If you have remaining doubts — add them now."

---

### Phase 3: Lead Synthesis

The facilitator (you) reads all revised conclusions and writes `{workspace}/04-synthesis.md`:

```markdown
# Facilitator Synthesis

## Consensus Zones
{Where all or majority agree — cite specific agents and arguments}

## Key Contradictions
{Where positions remain incompatible. Quote both sides.}

## Blind Spots
{What NOBODY mentioned but matters. Facilitator's own analysis.}

## Groupthink Check
{If everyone agrees on X — is this real consensus or politeness?
Did anyone CHANGE to agree, or did they always agree?}

## Questions for Final Round
- @{role}: {specific question about their position}
- @{role}: {specific question}
```

---

### Phase 4: Final Conclusions

Launch all agents in parallel:
```
Final round. Read the facilitator's synthesis:
{workspace}/04-synthesis.md

Pay attention to:
- Questions addressed to YOU specifically
- Blind spots identified
- Contradictions involving your position

Write your final conclusion to: {workspace}/05-final/{your_role}.md

Format: follow ./formats/final.md EXACTLY.
```

Wait for all agents.

---

### Phase 5: Consensus Document + JSON Output

#### 5a. Write consensus.md

Read all final conclusions. Write `{workspace}/05-final/consensus.md`:

```markdown
# Debate Consensus: {topic}

**Date:** {date}
**Participants:** {role_1}, {role_2}, ..., {role_N}
**Strategy:** adversarial | **Rounds:** {N}

## Executive Summary
{3-5 sentences capturing the core outcome}

## Agreed Recommendations
{What all participants support}
1. ...

## Conditional Recommendations
{Supported IF certain conditions hold}
1. ... (condition: ...)

## Contested Points
| Point | For (who + why) | Against (who + why) | Facilitator Assessment |
|-------|----------------|--------------------|-----------------------|

## Risk Matrix
| Risk | Probability | Impact | Mitigation | Source Agent |
|------|------------|--------|------------|-------------|

## Decision Framework (optional)
- If {condition A} → {recommendation X}
- If {condition B} → {recommendation Y}

## Open Questions (optional)
{What requires additional data to resolve}

## Key Insight from Debate Process (optional)
{What emerged ONLY because of adversarial debate — would not have surfaced in solo analysis}

## Argument Graph
{ASCII visualization of the debate flow. Auto-detect language from 00-brief.md.
Generate one tree per round showing: who pitched, who attacked, what was accepted/rejected.}

Example:
ROUND 1: {pitcher} pitches
├── {critic} ──FATAL FLAW──▶ "{flaw summary}" [{evidence_basis}, {confidence}/10]
│   └── {pitcher} rebuttal: {ACCEPTED|REJECTED} — {brief reason}
├── {critic} ──FATAL FLAW──▶ "{flaw}" [{basis}, {conf}/10] ★DA
│   └── {pitcher} rebuttal: {ACCEPTED|REJECTED} — {reason}
└── UPDATED THESIS: {new thesis after rebuttals}

---
*Generated with [debate-protocol](https://github.com/anthropics/debate-protocol)*
```

#### 5b. Write debate.result.json

Generate structured JSON output following `./debate.schema.json`. Include:
- All agent positions (initial and final)
- Confidence trajectories
- Fatal flaws with accepted/rejected status
- Consensus recommendations with confidence scores
- Quality metrics (positions changed, groupthink flag, etc.)

Write to `{workspace}/debate.result.json`.

---

### Phase 6: Cleanup

1. Report to user: "Debate complete. All artifacts: {workspace}/"
2. Highlight the consensus document and key insight
3. Mention debate.result.json for programmatic access

---

## Anti-Groupthink Mechanisms

Built into agent prompts and facilitator validation:

1. **Fatal Flaw mandatory** — every critique MUST contain a fatal flaw. "Generally agree" = failure.
2. **Pre-mortem mandatory** — every pitch MUST contain self-critique (2-3 ways thesis could be wrong).
3. **Devil's advocate rotation** — one critic per round attacks maximally hard. Each agent serves once.
4. **Disagreement scoring** — Agreement Map required in revised conclusions.
5. **Groupthink alarm** — if all agree on everything, facilitator challenges the converted agent.

## Formats

- `./formats/pitch.md` — Thesis + Evidence + Pre-mortem + Confidence
- `./formats/critique.md` — Fatal Flaw + Counter-scenario + DA mode
- `./formats/rebuttal.md` — Accepted/Defended + Updated Thesis
- `./formats/revised.md` — Agreement Map + Confidence Trajectory
- `./formats/final.md` — Evolution + Recommendations + What Would Change My Mind

## Data Schema

- `./debate.schema.json` — JSON schema for debate.result.json

## Presets

- `./presets/architecture.md` — Pragmatist, Purist, Operator
- `./presets/go-to-market.md` — Growth, Product, Finance, Customer
- `./presets/risk-assessment.md` — Optimist, Pessimist, Realist

## Templates

- `./templates/debater-prompt.md` — Base agent prompt
- `./templates/workspace.md` — Directory structure

## Cost Estimate

- **3 agents, 3 rounds:** ~$1-3 (30-50K tokens)
- **5 agents, 5 rounds:** ~$3-8 (80-150K tokens)

Actual cost depends on brief length, evidence depth, and model used.

## Common Mistakes

- **Parallel pitch rounds:** Rounds MUST be sequential — each round's context enriches the next
- **Skipping validation:** Always check rebuttals address each Fatal Flaw
- **Missing workspace:** Create directories before launching agents to write files
- **Forgetting brief:** 00-brief.md is shared context — without it agents lose problem framing
- **Same devil's advocate:** Rotate! Each agent should be DA exactly once
- **Bland translation:** English prompts must be commanding (MANDATORY, FORBIDDEN), not polite suggestions
