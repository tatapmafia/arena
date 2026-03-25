# Consensus Document: Open-Sourcing KubeDeploy

**Debate ID:** open-source-deploy-tool
**Date:** 2026-03-20
**Participants:** Strategist, Engineer, Finance

---

## Executive Summary

Open-source the deployment framework under Apache 2.0, but only after shipping KubeDeploy Cloud beta. Keep the ML-based canary scoring model proprietary. The value split is roughly 35% core / 65% platform, making open-source viable -- but sequencing is critical.

## Agreed Recommendations

All three agents agree on the following:

1. **The plugin architecture must exist before open-sourcing.** No exceptions. This isolates internal dependencies and controls the contribution surface.
2. **2 FTE minimum for community maintenance.** One hire, one internal rotation. Budget: ~$300K/yr fully loaded.
3. **4-month preparation timeline.** Includes: config service decoupling, plugin API, contributor guidelines, CI matrix, documentation.
4. **The canary scoring model (ML-based) stays proprietary.** The OSS version ships a pluggable canary interface with a basic threshold scorer.

## Contested Points

| Point | Strategist | Engineer | Finance |
|-------|-----------|----------|---------|
| License choice | Apache 2.0 | Apache 2.0 | Prefers BSL, accepts Apache if Cloud launches first |
| Fork risk severity | Low (survivable) | Medium | High (but mitigable with sequencing) |
| ROI timeline | 12 months | 18 months | 24 months |

## Conditional Recommendations

### IF >50% of KubeDeploy Cloud value is in the platform layer:
**Proceed with open-source.** The core becomes a distribution channel.

### IF >50% of value is in the deployment code itself:
**Do not open-source.** Offer a free tier of Cloud instead for community building.

### IF headcount freeze extends beyond Q2:
**Delay open-source until 2 FTE are secured.** Launching without community capacity will damage brand (abandoned-looking repo is worse than no repo).

## Risk Matrix

| Risk | Probability | Impact | Mitigation |
|------|------------|--------|------------|
| Competitor forks and ships hosted version | Medium (40%) | High | Launch Cloud beta before OSS release |
| Maintenance burden exceeds 2 FTE | High (60%) | Medium | Strict acceptance policy, plugin-only features |
| Community doesn't materialize | Low (20%) | Medium | KubeCon launch, blog series, partnership with CNCF |
| Internal team morale drop (OSS toil) | Medium (35%) | High | Rotation model, clear on-call boundaries |
| License drama (community demands AGPL/MIT) | Low (15%) | Low | Decide upfront, document reasoning, don't change |

## Decision Framework

```
1. Audit value split: core vs. platform (assign PM for 1-week analysis)
2. IF core > 50% value: STOP. Offer free Cloud tier instead.
3. IF platform > 50% value:
   a. Secure 2 FTE (1 hire + 1 rotation)
   b. Build plugin architecture (2 months)
   c. Ship KubeDeploy Cloud beta (month 3-4)
   d. Open-source core at KubeCon NA (month 5)
   e. Review at 6 months: community health, maintenance load, Cloud pipeline
```

## Key Insight

**Sequence matters more than the decision itself.** The debate revealed that "should we open-source?" is the wrong question. The right question is "in what order?" The answer:

1. Ship the hosted version first (establishes revenue and market position)
2. Then open-source the core (turns it into a distribution channel)
3. Never the reverse (giving away code before capturing value = the Redis trap)

This sequencing neutralizes Finance's fork concern and satisfies Strategist's hiring goals, while giving Engineering time to build the plugin architecture properly.

## Next Steps

- [ ] PM conducts value-split audit (1 week)
- [ ] Engineering scopes plugin architecture (2 weeks)
- [ ] Finance models Cloud pricing with OSS funnel assumptions
- [ ] Revisit this decision with audit data in 2 weeks
