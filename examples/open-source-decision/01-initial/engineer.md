# Initial Analysis: Engineer

## Position: Cautiously Against (Without More Resources)

### Maintenance Reality

Currently 1.5 engineers maintain KubeDeploy for 50 users who share our exact infrastructure. Open-sourcing means:
- Supporting N different Kubernetes versions (we run 1.28, community runs 1.24-1.30)
- CI/CD matrix explodes from 1 config to ~15
- Issue triage for environments we can't reproduce

**Estimate:** Maintenance load goes from 1.5 FTE to 3-4 FTE within 6 months of gaining traction. With frozen headcount, that's 40% of one team's capacity.

### Code Readiness

The codebase is solid internally but not OSS-ready:
- Hard-coded references to internal config service (3 packages)
- No plugin architecture for custom canary metrics
- Documentation is internal wiki pages, not standalone
- No contributor guidelines, governance model, or CoC

**Estimate:** 6-8 weeks of dedicated work to make it OSS-ready.

### What Could Change My Mind

- Dedicated community engineering hire (1 FTE)
- Plugin architecture that isolates our internal integrations
- Clear boundary: we maintain core, community maintains extensions

### Confidence: 60% against (without resources), 55% for (with dedicated hire)
