# debate-protocol

### Stop satisfying decisions. Start surviving them.

You're about to make a big call — rewrite the backend, raise a round, kill a product line. You ask AI for advice. It says "great idea!" You ask your team. Nobody wants to be the one to disagree. You ask a consultant. $500K and 6 weeks later, you get a PowerPoint.

**debate-protocol** makes AI agents **fight each other** over your decision. Mandatory fatal flaws. Forced devil's advocate. A groupthink alarm that fires when everyone suspiciously agrees. The output isn't validation — it's a stress test.

> *"I want to go to sleep knowing my decision survived an attack, not just received approval."*

---

## What You Get

```
WITHOUT debate-protocol              WITH debate-protocol
─────────────────────────────        ─────────────────────────────
"Claude, should we use Kafka?"       3 agents. 3 rounds. Every
                                     assumption attacked.
> "Kafka is a great choice
  for your use case because..."      > FATAL FLAW (from operator):
                                       Your team has 3 engineers.
                                       Kafka's operational overhead
                                       will consume 40% of sprint
                                       capacity for 6 months.
  (Confident. Unchallenged.
   Potentially expensive.)           Result: Decision Framework
                                     with conditions, risks, and
                                     the ONE insight that only
                                     emerged because someone was
                                     forced to attack it.
```

## Sample Output

Here's what the consensus document looks like (from [examples/open-source-decision](examples/open-source-decision/)):

```markdown
## Decision Framework
- If >50% of product value is in the hosting layer → open-source the core
- If >50% of value is in the code itself → keep proprietary
- In EITHER case → ship the hosted version BEFORE open-sourcing

## Key Insight from Debate Process
The strategist's "open-source for hiring" thesis survived all attacks
except one: finance's devil's advocate finding that a competitor could
fork and ship a hosted version in 2 weeks. This forced a sequencing
insight that didn't exist in any single agent's initial analysis:
the hosted version must ship FIRST, making open-source a distribution
channel rather than a giveaway.

## Contested Points
| Point              | For                    | Against               |
|--------------------|------------------------|-----------------------|
| Community ROI      | strategist: 3x hiring  | engineer: 40% time    |
|                    | pipeline improvement   | on external PRs       |
| Competitive risk   | strategist: moat is    | finance: fork risk    |
|                    | hosting, not code      | is real and immediate |
```

## Who This Is For

You don't need this for every decision. You need this for **the decisions that keep you up at night.**

- **"Should we rewrite or refactor?"** — your team agrees on rewrite. Nobody's challenging the 6-month estimate.
- **"Raise now or wait?"** — your advisor says raise. But they said that last time too.
- **"Kill the feature or double down?"** — sunk cost is whispering. Nobody's doing the math on opportunity cost.
- **"Hire senior or two juniors?"** — everyone has an opinion. Nobody has a framework.

If the cost of being wrong is low, just decide. If the cost is high — **make them fight first.**

## Install

```bash
# Copy to your Claude Code skills directory
git clone https://github.com/tatapmafia/arena.git
cp -r debate-protocol ~/.claude/skills/debate-protocol

# Or if you prefer a one-liner
git clone https://github.com/tatapmafia/arena.git ~/.claude/skills/debate-protocol
```

Requires: [Claude Code](https://claude.ai/claude-code)

## Usage

```bash
# Basic — define your own agents interactively
/debate "Should we rewrite our auth system?"

# With a preset — agents pre-configured for the decision type
/debate --preset architecture "Monolith or microservices?"
/debate --preset risk-assessment "Should we raise Series A now?"
/debate --preset go-to-market "Freemium or paid-only?"
```

## How It Works

```
Phase 0: Setup          Define agents, write brief, create workspace
    │
    ▼
Phase 1: Pitch Rounds   Each agent defends → others attack with Fatal Flaws
    │                    → speaker rebuts → facilitator validates
    │                    (one round per agent, sequential)
    ▼
Phase 2: Revised         All agents revise positions + Agreement Map
    │
    ▼
Phase 3: Synthesis       Facilitator identifies consensus, contradictions,
    │                    blind spots, groupthink
    ▼
Phase 4: Final           Agents write final conclusions addressing synthesis
    │
    ▼
Phase 5: Consensus       Structured consensus document + JSON output
```

## Anti-Groupthink Mechanisms

LLMs are sycophantic by default. debate-protocol forces genuine disagreement through 5 mechanisms:

- **Mandatory Fatal Flaw** — every critique MUST contain a fatal flaw. "I generally agree" = failure.
- **Mandatory Pre-mortem** — every pitch MUST include 2-3 reasons the author could be wrong.
- **Devil's Advocate Rotation** — one critic per round attacks as hard as possible, even if they agree.
- **Disagreement Scoring** — Agreement Map required: who you agree with, who you don't, and why.
- **Groupthink Alarm** — if all agents suddenly agree on everything, facilitator challenges the conversion.

## Presets

| Preset | Agents | Best For |
|--------|--------|----------|
| **architecture** | Pragmatist, Purist, Operator | Build vs buy, tech selection, rewrite vs refactor |
| **go-to-market** | Growth, Product, Finance, Customer | Launch strategy, pricing, market entry |
| **risk-assessment** | Optimist, Pessimist, Realist | Investment, go/no-go, any high-stakes bet |

**Custom presets:** Drop a `.md` file in `presets/`. See [presets/architecture.md](presets/architecture.md) for the format.

## Output

Every debate produces two outputs:

1. **`consensus.md`** — Human-readable decision document with recommendations, contested points, risk matrix, and decision framework
2. **`debate.result.json`** — Machine-readable structured data following [debate.schema.json](debate.schema.json). Queryable with `jq`, integrable with CI/CD.

```bash
# What were the P0 recommendations?
jq '.consensus.recommendations[] | select(.priority=="P0")' debate.result.json

# Did any agent change their position?
jq '.agents[] | select(.position_changed) | {role, change_trigger}' debate.result.json

# Was there a groupthink flag?
jq '.quality_metrics.groupthink_flag' debate.result.json
```

## Cost

| Setup | Estimated Cost | Time |
|-------|---------------|------|
| 3 agents, 3 rounds | ~$1-3 | ~15 min |
| 5 agents, 5 rounds | ~$3-8 | ~30 min |

Actual cost depends on brief length, evidence depth, and model used.

## Project Structure

```
debate-protocol/
├── SKILL.md                 # Core protocol (facilitator instructions)
├── debate.schema.json       # JSON schema for structured output
├── formats/                 # Document format templates
│   ├── pitch.md             # Thesis + Evidence + Pre-mortem
│   ├── critique.md          # Fatal Flaw + Counter-scenario
│   ├── rebuttal.md          # Accept/Defend + Updated Thesis
│   ├── revised.md           # Agreement Map + Confidence Trajectory
│   └── final.md             # Evolution + Recommendations
├── presets/                 # Pre-configured agent sets
│   ├── architecture.md
│   ├── go-to-market.md
│   └── risk-assessment.md
├── templates/               # Agent prompt templates
│   ├── debater-prompt.md
│   └── workspace.md
├── examples/                # Worked examples
│   └── open-source-decision/
└── LICENSE
```

## Contributing

- **New presets:** PR a `.md` file to `presets/` for decision types we don't cover yet
- **Example debates:** Run a debate, anonymize it, PR to `examples/`
- **Format improvements:** Better document structures for specific use cases
- **Bug reports:** Open an issue if agents produce weak output — include the workspace files

## Roadmap

- **v2.0** (current) — Standalone mode, JSON output, presets, English-first
- **v2.1** — Plugin strategies (red-team, dialectic, delphi), debate comparison tool
- **v3.0** — Multi-model debates (Claude vs GPT vs Gemini), GitHub Action integration

## License

MIT — see [LICENSE](LICENSE)

---

*The best decision isn't the one everyone agrees with. It's the one that survived the attack.*
