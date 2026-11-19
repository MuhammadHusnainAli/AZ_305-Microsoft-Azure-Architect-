# Q&A: Fundamentals, CAF and Well-Architected Framework

## Fundamentals
**Q1.** Region vs availability zone vs region pair?
<details><summary>Answer</summary>Region = geographic area of datacenters. Zone = separate datacenter(s) inside a region (≥3). Pair = two regions in the same geography for sequential updates and prioritised recovery.</details>

**Q2.** Subscription is a boundary for what?
<details><summary>Answer</summary>**Billing** and **access control** (and quotas).</details>

**Q3.** LRS vs ZRS vs GRS vs GZRS?
<details><summary>Answer</summary>LRS: 3 copies, 1 DC. ZRS: 3 zones. GRS: LRS + async LRS copy in paired region. GZRS: ZRS + LRS copy in paired region. RA-: readable secondary.</details>

**Q4.** Owner vs Contributor vs User Access Administrator?
<details><summary>Answer</summary>Owner: everything incl. access. Contributor: everything except granting access. UAA: manage access only.</details>

**Q5.** Defense-in-depth layers?
<details><summary>Answer</summary>Physical → Identity & access → Perimeter → Network → Compute → Application → Data.</details>

**Q6.** Zero Trust principles?
<details><summary>Answer</summary>Verify explicitly, least privilege, assume breach.</details>

## Cloud Adoption Framework
**Q7.** CAF phases in order?
<details><summary>Answer</summary>Strategy → Plan → Ready → Adopt (Migrate / Modernize / Cloud-native), with Govern, Manage, Secure throughout.</details>

**Q8.** What is an Azure landing zone?
<details><summary>Answer</summary>A pre-configured, scalable environment (MG hierarchy, subscriptions, identity, networking, policy, management) where workloads are deployed. Platform LZ + application LZs.</details>

**Q9.** Rehost vs replatform vs refactor vs rearchitect?
<details><summary>Answer</summary>Rehost: VM lift-and-shift. Replatform: minor change to a managed platform. Refactor: code changes for PaaS. Rearchitect: change architecture (microservices).</details>

**Q10.** CAF vs WAF?
<details><summary>Answer</summary>CAF = organisation's cloud journey. WAF = quality of an individual workload.</details>

## Well-Architected pillars
**Q11.** Name the 5 pillars.
<details><summary>Answer</summary>Reliability, Security, Cost Optimization, Operational Excellence, Performance Efficiency.</details>

**Q12.** Retry, circuit breaker, bulkhead belong to?
<details><summary>Answer</summary>**Reliability.**</details>

**Q13.** Which service injects faults to validate resilience?
<details><summary>Answer</summary>**Azure Chaos Studio.**</details>

**Q14.** Security pillar: three things you protect?
<details><summary>Answer</summary>Confidentiality, integrity, availability (CIA).</details>

**Q15.** Cost: reservation vs savings plan vs Spot?
<details><summary>Answer</summary>Reservation: specific SKU/region, steady use. Savings plan: hourly spend commitment, flexible. Spot: deep discount, can be evicted.</details>

**Q16.** How do you hold teams accountable for spend?
<details><summary>Answer</summary>**Tags** + Cost Management cost analysis/budgets per owner, enforced by Policy.</details>

**Q17.** Safe deployment practices?
<details><summary>Answer</summary>Small changes, progressive exposure (rings/canary), health gates, automatic rollback, feature flags, deployment slots.</details>

**Q18.** Scale up vs scale out?
<details><summary>Answer</summary>Up = bigger instance (limit, maybe downtime). Out = more instances (needs stateless; preferred).</details>

**Q19.** Prove the app meets 2-second response at 5,000 users before launch.
<details><summary>Answer</summary>**Azure Load Testing** in the CI/CD pipeline with pass/fail criteria.</details>

**Q20.** Tool that gives recommendations across all five pillars for deployed resources.
<details><summary>Answer</summary>**Azure Advisor.**</details>

## More fundamentals
**Q21.** Shared responsibility: in PaaS, who patches the OS?
<details><summary>Answer</summary>**Microsoft.** You own data, identities, access, and app config.</details>

**Q22.** In IaaS, who is responsible for OS patches and firewall rules on the VM?
<details><summary>Answer</summary>**You (customer).**</details>

**Q23.** CapEx vs OpEx?
<details><summary>Answer</summary>CapEx: upfront spend on physical assets. OpEx: pay-as-you-go for services consumed (cloud).</details>

**Q24.** Scalability vs elasticity?
<details><summary>Answer</summary>Scalability = ability to handle more load by adding resources. Elasticity = automatically adding/removing resources as demand changes.</details>

**Q25.** Which services are global (not regional)?
<details><summary>Answer</summary>Examples: Microsoft Entra ID, Traffic Manager, Front Door, Azure DNS (public zones).</details>

**Q26.** Azure Arc purpose?
<details><summary>Answer</summary>Manage servers, Kubernetes and data services outside Azure (on-prem, other clouds) with Azure tools: Policy, RBAC, Monitor, Defender.</details>

**Q27.** Purpose of Azure Advisor's five categories?
<details><summary>Answer</summary>Reliability, Security, Performance, Cost, Operational Excellence — same as WAF pillars.</details>

**Q28.** Pricing calculator vs TCO calculator?
<details><summary>Answer</summary>Pricing calculator: estimate Azure costs of planned services. TCO calculator: compare on-prem costs vs Azure.</details>

## More CAF
**Q29.** Which CAF methodology defines business motivations and expected outcomes?
<details><summary>Answer</summary>**Strategy.**</details>

**Q30.** Who usually forms a cloud center of excellence (CCoE) / cloud platform team?
<details><summary>Answer</summary>Cross-functional experts who build the landing zones, standards and guardrails (Ready/Govern), enabling workload teams.</details>

**Q31.** Five disciplines of cloud governance (classic CAF)?
<details><summary>Answer</summary>Cost management, security baseline, resource consistency, identity baseline, deployment acceleration.</details>

**Q32.** Landing zone accelerator — what does it deploy?
<details><summary>Answer</summary>MG hierarchy, policies, platform subscriptions (identity, management, connectivity), hub networking, logging, Defender — following CAF design areas.</details>

**Q33.** What is a "workload" vs "platform" team?
<details><summary>Answer</summary>Platform team runs shared services and guardrails; workload teams build and run applications in application landing zones.</details>

## More WAF
**Q34.** Reliability: what is a failure mode analysis (FMA)?
<details><summary>Answer</summary>Identify how each component can fail, the impact, and mitigations (retries, redundancy, fallbacks).</details>

**Q35.** Reliability: what is graceful degradation?
<details><summary>Answer</summary>Keep core functions running with reduced features when a dependency fails (e.g. show cached data).</details>

**Q36.** Reliability: SLO vs SLA vs SLI?
<details><summary>Answer</summary>SLI = measured metric (e.g. success rate). SLO = internal target for the SLI. SLA = contractual commitment with penalties.</details>

**Q37.** Security: what is threat modelling?
<details><summary>Answer</summary>Systematically identify threats (e.g. STRIDE), attack surfaces and mitigations during design.</details>

**Q38.** Security: what is "least privilege" in practice?
<details><summary>Answer</summary>Narrow-scoped roles, JIT with PIM, managed identities with only needed data roles, no standing admin access.</details>

**Q39.** Security: how do you protect against data exfiltration from PaaS?
<details><summary>Answer</summary>Private endpoints, disable public access, service endpoint policies, Azure Firewall egress control, Purview DLP.</details>

**Q40.** Cost: what is FinOps?
<details><summary>Answer</summary>A practice joining finance, engineering and business to own cloud spend: inform → optimise → operate.</details>

**Q41.** Cost: how do you find idle resources?
<details><summary>Answer</summary>**Azure Advisor** cost recommendations, Cost Management, Resource Graph queries for unattached disks/public IPs.</details>

**Q42.** Cost: when is serverless more expensive than provisioned?
<details><summary>Answer</summary>When usage is high and constant; at steady high load, reserved/provisioned capacity is cheaper.</details>

**Q43.** Operational excellence: the DORA metrics?
<details><summary>Answer</summary>Deployment frequency, lead time for changes, change failure rate, time to restore service.</details>

**Q44.** Operational excellence: what is a runbook?
<details><summary>Answer</summary>Documented or automated steps to perform an operational task or respond to an incident (e.g. Azure Automation runbook).</details>

**Q45.** Performance: what is the noisy neighbour problem?
<details><summary>Answer</summary>One tenant/workload consumes shared resources and degrades others; mitigate with throttling, isolation, separate pools/stamps.</details>

**Q46.** Performance: CQRS?
<details><summary>Answer</summary>Separate read and write models/stores so each can scale and be optimised independently.</details>

**Q47.** Performance: where should caching be applied first?
<details><summary>Answer</summary>Hot, read-heavy, rarely changing data on critical paths (static content at the edge, reference data in Redis).</details>

**Q48.** Which pillar includes "sustainability" guidance?
<details><summary>Answer</summary>WAF has a separate **sustainable workloads** guidance that relates to cost and performance efficiency (use less, scale to zero, efficient regions).</details>
