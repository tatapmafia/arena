# Round 1 Pitch: Open-Source KubeDeploy

**Pitcher:** Strategist
**Position:** Open-source the core deployment engine

---

## Thesis

Open-sourcing KubeDeploy is not charity -- it is the fastest path to market dominance for KubeDeploy Cloud.

## Evidence

### 1. The HashiCorp Playbook Works

| Company | OSS Project | Hosted Revenue | Time to $100M ARR |
|---------|------------|----------------|---------------------|
| HashiCorp | Terraform | Terraform Cloud | 4 years |
| Elastic | Elasticsearch | Elastic Cloud | 3.5 years |
| GitLab | GitLab CE | GitLab.com | 5 years |

Every successful infrastructure company in the last decade followed: **OSS core -> hosted premium**.

### 2. Hiring Impact Is Measurable

Datapoint: Lyft open-sourced Envoy in 2016. Their infrastructure team's inbound applications increased 4x within 12 months. We need 15 engineers this year in a market where the average infrastructure hire takes 90 days.

### 3. The Moat Is in the Platform, Not the Code

KubeDeploy Cloud's value will be:
- Multi-tenant deployment orchestration
- Audit trails and compliance dashboards
- SSO/RBAC integration
- SLA-backed uptime

None of this is in the open-source core. The core is the **distribution channel**, not the product.

## Proposed Sequencing

1. **Month 1-2:** Decouple internal config service, add plugin architecture
2. **Month 3:** Soft launch at KubeCon EU (talk + repo)
3. **Month 4-6:** Build community, collect enterprise feature requests
4. **Month 7:** Launch KubeDeploy Cloud beta with OSS adopters as pipeline

## Ask

Approve open-sourcing with one dedicated community engineer hire and a 2-month prep window.
