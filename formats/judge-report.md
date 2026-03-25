# Judge Report Format

The Judge is a separate agent that evaluates the debate AFTER all rounds complete. The Judge does not participate in the debate — they observe and assess.

```markdown
# Judge Report

## Overall Assessment
{One paragraph: was this a high-quality debate? Did agents genuinely engage or go through the motions?}

## Agent Evaluation

### {role_1}
- **Argument quality:** {1-10} — {brief justification}
- **Evidence strength:** {1-10} — {cited data vs speculation}
- **Intellectual honesty:** {1-10} — {changed position when warranted? acknowledged weaknesses?}
- **Key contribution:** {the one thing this agent added that others didn't}

### {role_2}
(same format)

### {role_N}
(same format)

## Fatal Flaw Analysis
| Flaw | Author | Target | Evidence Basis | Accepted? | Impact on Final Decision |
|------|--------|--------|---------------|-----------|------------------------|
| {flaw} | {who wrote it} | {whose pitch} | OBSERVED/INFERRED/SPECULATIVE | Yes/No | High/Medium/Low |

## Strongest Arguments (survived all attacks)
1. {argument} — by {role}, attacked by {roles}, survived because {reason}

## Weakest Arguments (collapsed under scrutiny)
1. {argument} — by {role}, killed by {role}'s fatal flaw: {flaw}

## Groupthink Assessment
{Did agents genuinely disagree or converge too quickly?
Any signs of fabricated critiques (SPECULATIVE + low confidence)?
Any positions that changed without strong justification?}

## Debate Quality Score: {1-10}
{Justification. What would have made it better?}
```
