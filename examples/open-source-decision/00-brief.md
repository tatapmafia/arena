# Debate Brief: Open-Sourcing the Deployment Tool

## Context

We built **KubeDeploy** internally 18 months ago. It handles blue-green deployments on Kubernetes with zero-downtime rollbacks, canary analysis, and automated health checks.

## Current State

- **Users:** 50 internal engineers across 4 teams
- **Codebase:** ~12K lines of Go, well-tested (87% coverage)
- **Dependencies:** Kubernetes API, Prometheus for canary metrics, internal config service
- **Alternatives:** Argo Rollouts, Flagger, Spinnaker (all lack our canary scoring model)
- **Maintenance:** 1.5 engineers maintaining it part-time

## The Question

Should we open-source KubeDeploy?

## Constraints

- Engineering headcount is frozen for Q2
- We're hiring 15 engineers this year (competitive market)
- Two competitors have expressed interest in our deployment approach at KubeCon
- Our hosted platform (KubeDeploy Cloud) is in early planning

## Success Criteria for This Debate

1. Clear recommendation with conditions
2. Risk matrix for both paths
3. Sequencing plan if we proceed
