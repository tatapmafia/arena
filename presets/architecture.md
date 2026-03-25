---
name: architecture
type: preset
description: Technical architecture decisions — build vs buy, monolith vs micro, rewrite vs refactor
recommended_strategy: adversarial
agents:
  - role: pragmatist
    brief: "Experienced tech lead. Values shipping speed, team productivity, and proven technology. Asks: 'Will this actually work with our team and timeline?'"
  - role: purist
    brief: "Staff engineer / architect. Values clean design, scalability, and long-term maintainability. Asks: 'Will we regret this in 2 years?'"
  - role: operator
    brief: "SRE / platform engineer. Values operational simplicity, observability, and reliability. Asks: 'Who gets paged at 3am when this breaks?'"
---

# Architecture Preset

Best for: build vs buy, monolith vs microservices, rewrite vs refactor, technology selection, migration decisions.

## Why these 3 roles

Architecture decisions fail when they optimize for only one dimension. The pragmatist prevents over-engineering, the purist prevents tech debt, the operator prevents operational nightmares. The tension between them surfaces trade-offs that a single perspective misses.

## Example questions

- "Should we rewrite the auth service or patch it?"
- "Monolith or microservices for a 15-person eng team?"
- "Build custom analytics or buy Amplitude?"
- "Migrate to Kubernetes or stay on EC2?"
- "GraphQL or REST for our public API?"

## Calibration notes

- If your team is <5 engineers, the operator role may overlap with pragmatist. Consider replacing operator with "customer-advocate" (someone who represents end-user impact).
- For security-critical decisions, add a 4th agent: "security-engineer".
