# Becoming a great Azure Architect: interview and real-world Q&A

**Q1.** How do you start designing any solution?
<details><summary>Answer</summary>Gather **business requirements** (outcomes, users, SLAs, RTO/RPO, compliance, budget, growth), list **constraints** (skills, existing licences, timelines), identify **critical flows**, then choose services per WAF pillar and document trade-offs as **Architecture Decision Records (ADRs)**.</details>

**Q2.** IaaS vs PaaS — how do you decide?
<details><summary>Answer</summary>Prefer PaaS for less operations, built-in HA and scaling. Choose IaaS when you need OS control, unsupported software, specific licensing, or a fast lift-and-shift.</details>

**Q3.** How do you design for multi-tenancy?
<details><summary>Answer</summary>Pick an isolation model per layer: shared everything (cheapest), database per tenant, or deployment stamps per tenant group. Consider noisy neighbours, data isolation, per-tenant cost tracking and onboarding automation.</details>

**Q4.** How do you choose between single-region with zones and multi-region?
<details><summary>Answer</summary>Compare the business cost of downtime with the cost of complexity. Zones cover datacenter failures cheaply. Multi-region is needed when RTO/RPO demand surviving a region loss, or users are global. Consider active-passive (cheaper) vs active-active (lower RTO, data consistency challenges).</details>

**Q5.** What is the difference between availability and resiliency?
<details><summary>Answer</summary>Availability = proportion of time the system is usable. Resiliency = ability to recover from failures and continue functioning.</details>

**Q6.** How do you secure a three-tier web app on Azure?
<details><summary>Answer</summary>Front Door + WAF → App Service/AKS with private endpoints and VNet integration → SQL with private endpoint, Entra auth, TDE. Managed identities + Key Vault, NSGs, Azure Firewall for egress, Defender for Cloud, logging to Log Analytics/Sentinel, DDoS protection, least-privilege RBAC with PIM.</details>

**Q7.** How do you control cloud costs as an architect?
<details><summary>Answer</summary>Right-size from metrics, autoscale, shut down non-prod, reservations/savings plans for baseline, Spot for interruptible, Hybrid Benefit, storage tiering, tagging + budgets + showback, review Advisor monthly, design for cost per transaction.</details>

**Q8.** Monolith to microservices — how?
<details><summary>Answer</summary>**Strangler fig** pattern: put a gateway in front, extract bounded contexts one at a time, give each its own data store, communicate asynchronously via messaging, add observability and CI/CD per service.</details>

**Q9.** Synchronous vs asynchronous communication?
<details><summary>Answer</summary>Sync (HTTP) is simpler but couples availability and latency. Async (queues/events) decouples, levels load and improves resilience, at the cost of eventual consistency and complexity.</details>

**Q10.** What patterns do you know and when do you use them?
<details><summary>Answer</summary>Retry, circuit breaker, bulkhead (reliability); cache-aside, CQRS, sharding, materialized view (performance); gateway offloading/routing (API management); strangler fig (migration); valet key (SAS for direct storage access); claim check (large messages in storage, reference in queue); competing consumers; queue-based load leveling; deployment stamps; sidecar/ambassador.</details>

**Q11.** How do you handle secrets in CI/CD?
<details><summary>Answer</summary>No secrets in repos. Workload identity federation for deployments, Key Vault references for apps, managed identities at runtime, secret scanning in pipelines.</details>

**Q12.** How do you plan IP addressing for Azure?
<details><summary>Answer</summary>Non-overlapping ranges with on-prem and across all VNets, room for growth, dedicated subnets (GatewaySubnet, AzureFirewallSubnet, AzureBastionSubnet), size subnets for AKS/App Service integration, managed with IPAM in AVNM.</details>

**Q13.** How do you measure whether your architecture is good?
<details><summary>Answer</summary>Run Well-Architected Reviews, track SLOs/error budgets, cost per unit, deployment frequency/lead time/change failure rate/MTTR (DORA), Secure Score, and Advisor score.</details>

**Q14.** A stakeholder wants 100% uptime. What do you say?
<details><summary>Answer</summary>Explain that 100% is not achievable and that each extra "nine" multiplies cost. Quantify the business impact of downtime, then agree a realistic SLO with RTO/RPO and a cost for each option.</details>

**Q15.** How do you document architecture?
<details><summary>Answer</summary>Context and container diagrams (C4), data flows, network topology, ADRs with trade-offs, non-functional requirements, runbooks and DR plans; keep them in source control next to IaC.</details>

**Q16.** What soft skills matter most?
<details><summary>Answer</summary>Translating business needs to technical options, explaining trade-offs simply, influencing without authority, writing clearly, and continuous learning (Azure updates, Architecture Center).</details>
