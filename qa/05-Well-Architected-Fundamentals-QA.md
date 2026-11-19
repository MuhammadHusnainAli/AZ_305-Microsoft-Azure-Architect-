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
