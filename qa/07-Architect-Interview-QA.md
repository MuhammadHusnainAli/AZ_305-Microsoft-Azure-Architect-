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

**Q17.** Walk me through a request from a browser to a database in a secure Azure web app.
<details><summary>Answer</summary>DNS → Front Door (TLS, WAF, caching) → Private Link/App Gateway → App Service/AKS (managed identity) → Key Vault for config secrets → Azure SQL via private endpoint with Entra auth → telemetry to Application Insights/Log Analytics.</details>

**Q18.** How do you decide between AKS and Container Apps?
<details><summary>Answer</summary>Container Apps when the team wants serverless containers with little platform ops. AKS when you need full Kubernetes API control, custom operators, service mesh, specific networking or node-level tuning, and have the skills to run it.</details>

**Q19.** How do you design for idempotency and why?
<details><summary>Answer</summary>Messages and retries can be delivered more than once. Use unique operation IDs, upserts, dedup tables or Service Bus duplicate detection so repeat processing gives the same result.</details>

**Q20.** Eventual consistency — how do you explain it to the business?
<details><summary>Answer</summary>Data reaches every copy shortly after a write, not instantly. In exchange the system is faster and more available. Pick where it is acceptable (e.g. product reviews) and where it is not (e.g. account balance).</details>

**Q21.** How do you handle a distributed transaction across microservices?
<details><summary>Answer</summary>Avoid 2PC; use the **Saga** pattern (choreography with events or orchestration with Durable Functions) with **compensating transactions**, plus the **outbox** pattern for reliable publishing.</details>

**Q22.** What are deployment stamps and when would you use them?
<details><summary>Answer</summary>Independent copies of the whole workload (scale units). Use to scale beyond service limits, isolate tenants, or deploy close to users per region.</details>

**Q23.** How do you approach a performance problem in production?
<details><summary>Answer</summary>Measure first (App Insights, metrics), find the bottleneck (DB queries, dependencies, CPU, network), fix the biggest one, load test, and set alerts/SLOs to prevent regression.</details>

**Q24.** How do you prevent a single bad deployment from taking down every region?
<details><summary>Answer</summary>Progressive rollout: one region/ring at a time, health gates, automatic rollback, feature flags, and keep regions independent (no shared single point).</details>

**Q25.** What would you put in a landing zone for a new application team?
<details><summary>Answer</summary>Subscription under the right MG, spoke VNet peered to hub, policies (regions, tags, diagnostics), RBAC groups with PIM, budget alerts, Log Analytics connection, Defender plans, CI/CD identity.</details>

**Q26.** How do you secure the software supply chain?
<details><summary>Answer</summary>Protected branches and reviews, dependency/secret scanning, signed artifacts and images, private registries, least-privilege pipeline identities (OIDC), Defender for DevOps/containers.</details>

**Q27.** How do you decide between building and buying (SaaS)?
<details><summary>Answer</summary>Buy when the capability is not a differentiator and a SaaS meets requirements; build when it is core to competitive advantage or no product fits. Consider total cost, lock-in, compliance.</details>

**Q28.** What is technical debt in architecture and how do you manage it?
<details><summary>Answer</summary>Shortcuts that make future change harder. Make it visible (ADRs, backlog), prioritise by risk/cost, and pay it down continuously.</details>

**Q29.** How do you get buy-in for a costly reliability improvement?
<details><summary>Answer</summary>Quantify cost of downtime (revenue/hour, SLA penalties, reputation) vs. cost of the solution; show options with RTO/RPO and price; let the business choose.</details>

**Q30.** How do you keep up to date with Azure?
<details><summary>Answer</summary>Azure Updates, Azure Architecture Center, Well-Architected guides, Microsoft Learn, Azure Friday, hands-on labs and side projects.</details>

**Q31.** Explain the difference between a load balancer, an API gateway and a service mesh.
<details><summary>Answer</summary>Load balancer distributes traffic (L4/L7). API gateway manages north-south APIs (auth, quotas, transformation). Service mesh manages east-west service-to-service traffic (mTLS, retries, observability) inside a cluster.</details>

**Q32.** How do you design an application for zero-downtime database schema changes?
<details><summary>Answer</summary>Expand-and-contract: add new columns/tables first (backward compatible), deploy code that writes both, migrate data, switch reads, then remove old structures later.</details>

**Q33.** What questions do you ask before choosing a database?
<details><summary>Answer</summary>Data model (relational, document, graph, time-series), consistency needs, read/write ratio, scale, latency, global distribution, query patterns, team skills, compliance, cost.</details>

**Q34.** How do you design logging so it is useful during an incident?
<details><summary>Answer</summary>Structured logs with correlation IDs, consistent severity, central workspace, dashboards of golden signals (latency, traffic, errors, saturation), actionable alerts linked to runbooks.</details>

**Q35.** What makes a good architecture decision record (ADR)?
<details><summary>Answer</summary>Context, decision, options considered, trade-offs, consequences, status and date — short and kept with the code.</details>
