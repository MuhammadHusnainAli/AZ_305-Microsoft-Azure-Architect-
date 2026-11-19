# Case Studies (exam-style)

The exam gives a long scenario followed by several questions. Practise extracting **requirements → constraints → service**.

---
## Case 1: Contoso Retail
**Scenario.** Contoso has 3 on-prem datacenters (VMware), AD DS with 5,000 users, a .NET e-commerce site with SQL Server 2019 (uses SQL Agent and cross-database queries), 200 TB of product images on file servers, and a nightly SSIS ETL. They are expanding to Europe and Asia.

**Requirements**
1. Users sign in with on-prem credentials; sign-in must keep working if datacenters go down.
2. Admin rights only on request, time-limited.
3. Site must stay up if an Azure region fails; global users need low latency and WAF.
4. Database: PaaS, minimal code changes, RPO < 5 s across regions, no connection-string changes.
5. Product images: move quickly despite 200 Mbps link; then served globally.
6. Reuse SSIS packages.
7. Developers must not store secrets in code.
8. All resources must be tagged with `CostCenter`; deny deployments outside approved regions.

<details><summary>Answers</summary>

| # | Solution |
|---|---|
| 1 | Entra Connect/Cloud Sync with **password hash sync** + Seamless SSO |
| 2 | **PIM** (Entra ID P2) |
| 3 | App Service (Premium v3, zone redundant) in 2+ regions behind **Front Door Premium with WAF** |
| 4 | **SQL Managed Instance** + **auto-failover group** (listener endpoint) |
| 5 | **Data Box Heavy / multiple Data Boxes** offline → Blob storage → **Front Door/CDN** |
| 6 | **ADF Azure-SSIS IR** (self-hosted IR if sources stay on-prem) |
| 7 | **Managed identities + Key Vault** |
| 8 | **Azure Policy** at management group: require/inherit tag (Modify) + allowed locations (Deny) |
</details>

---
## Case 2: Fabrikam Manufacturing (IoT)
**Scenario.** 20,000 machines send telemetry every second. Engineers want a live dashboard, alerts within 10 seconds of a fault, and 5 years of history for ML. Plant networks connect to Azure privately. The security team uses a third-party SIEM.

<details><summary>Answers</summary>

- Ingestion: **IoT Hub** (device management) or **Event Hubs**.
- Hot path: **Stream Analytics** → Power BI + alerts (Functions/Logic Apps).
- Cold path: Event Hubs **Capture** → **ADLS Gen2** → **Databricks** for ML.
- Interactive telemetry analytics: **Azure Data Explorer**.
- Private connectivity: **ExpressRoute** + **private endpoints**.
- SIEM: diagnostic settings → **Event Hubs** → SIEM.
</details>

---
## Case 3: Litware Financial
**Scenario.** Regulated bank. Customer data must stay in-country, DBAs must not see account numbers, backups must be tamper-proof, all PaaS must be private, and auditors need 10 years of database backups and 7 years of logs.

<details><summary>Answers</summary>

- Residency: **Azure Policy allowed locations**; storage **ZRS** (not GRS if the pair is out-of-country); SQL backup redundancy **ZRS/LRS**.
- **Always Encrypted** for account numbers; TDE with **CMK in Managed HSM**.
- **Immutable vault** + soft delete + MUA for Azure Backup.
- **Private endpoints** + disabled public access + DNS Private Resolver.
- **SQL LTR 10 years**; logs to **storage with immutability** (7 years) and Log Analytics for active queries.
- **Defender for Cloud** regulatory compliance + **Sentinel**.
</details>

---
## Case 4: Startup SaaS
**Scenario.** Multi-tenant SaaS, 500 tenants with unpredictable usage, small team, wants low ops, microservices that scale to zero, async order processing with retries, and gradual feature rollout.

<details><summary>Answers</summary>

- Compute: **Azure Container Apps**.
- Data: **SQL Database elastic pool** (database per tenant) or Cosmos DB with `tenantId` partition key.
- Messaging: **Service Bus** queues/topics with dead-lettering.
- API front door: **API Management** (Consumption/v2).
- Feature rollout: **App Configuration feature flags**.
- CI/CD: **GitHub Actions + Bicep** with workload identity federation.
- Cost: budgets, autoscale, serverless where possible.
</details>

---
## Case 5: Northwind Healthcare
**Scenario.** Hospital group with 12 sites. Patient records in SQL Server 2017 (uses CLR and Service Broker). Medical images (DICOM) of 800 TB growing 50 TB/year, rarely read after 90 days. Doctors use a web portal from anywhere. HIPAA compliance. Sites connect over MPLS today.

**Requirements**
1. Database PaaS with no code changes, private network only.
2. Images: lowest cost, kept 10 years, immutable.
3. Doctors sign in with MFA only from compliant devices.
4. Connect 12 sites to Azure with minimal management.
5. Prove compliance continuously.
6. RTO 4 hours, RPO 15 minutes for the database if a region fails.

<details><summary>Answers</summary>

| # | Solution |
|---|---|
| 1 | **SQL Managed Instance** (CLR, Service Broker) in a VNet |
| 2 | Blob storage, **lifecycle** Hot → Cool → Archive after 90 days, **immutable (WORM) policy**; initial upload with **Data Box Heavy** |
| 3 | **Conditional Access** require MFA + **compliant device** (Intune) |
| 4 | **Azure Virtual WAN** with site-to-site VPN/SD-WAN (or ExpressRoute) |
| 5 | **Defender for Cloud regulatory compliance (HIPAA)** + Azure Policy initiative |
| 6 | **Auto-failover group** for SQL MI to a secondary region |
</details>

---
## Case 6: Adventure Works Gaming
**Scenario.** Online game with 5 million players worldwide. Leaderboards update in real time, player profiles are JSON, matchmaking servers use UDP, and traffic spikes 20× on weekends.

<details><summary>Answers</summary>

- Player profiles: **Cosmos DB** multi-region writes, partition by playerId.
- Leaderboards: **Azure Cache for Redis** (sorted sets), Enterprise active geo-replication.
- Game servers (UDP): **AKS** or VMSS per region + **Traffic Manager** (performance routing; Front Door is HTTP only) or cross-region Load Balancer.
- Spikes: **autoscale** (VMSS / cluster autoscaler), Spot for non-critical workloads.
- Telemetry: **Event Hubs** → Stream Analytics / Data Explorer.
- Web/API: **Front Door + WAF** → Container Apps/App Service.
</details>

---
## Case 7: Woodgrove Bank — hybrid identity and access
**Scenario.** 20,000 users in AD DS, two forests after a merger. Security accepts password hashes in the cloud but not on-prem dependencies for sign-in. Remote branches have unreliable links to the datacenter. Contractors need 6-month access. Admins currently have permanent Global Admin.

<details><summary>Answers</summary>

- Sync both forests: **Entra Cloud Sync** (or Connect Sync) to one tenant.
- Auth: **password hash sync** (works even when links to on-prem fail) + Seamless SSO; add leaked-credential detection.
- Contractors: **B2B guests** or member accounts via **access packages** with 6-month expiry + **access reviews**.
- Admins: **PIM** eligible assignments, approval, MFA; break-glass accounts.
- Risk: **Identity Protection** risk-based Conditional Access.
</details>

---
## Case 8: Tailspin Analytics platform
**Scenario.** Data from 40 on-prem SQL databases, SaaS apps (Salesforce) and clickstream must land in a central lake. Analysts use SQL; data scientists use Python. Executives want dashboards refreshed every 15 minutes. Data must be catalogued and classified.

<details><summary>Answers</summary>

- Batch ingestion: **Data Factory** (self-hosted IR for on-prem, Salesforce connector).
- Streaming clickstream: **Event Hubs** → Stream Analytics / Databricks streaming.
- Storage: **ADLS Gen2**, medallion layers in Delta.
- Analysts: **Synapse serverless/dedicated SQL** (or Fabric Warehouse).
- Data science: **Databricks**.
- Dashboards: **Power BI** (DirectQuery/incremental refresh).
- Governance: **Microsoft Purview**.
</details>

---
## Case 9: Fourth Coffee — cost-constrained migration
**Scenario.** 300 VMs on VMware, many idle at night. Windows Server and SQL licences with Software Assurance. CFO wants lowest 3-year cost; dev/test VMs only run in office hours.

<details><summary>Answers</summary>

- Assess with **Azure Migrate** (performance-based sizing, business case).
- Production: **Reserved Instances/Savings plan** + **Azure Hybrid Benefit** for Windows and SQL.
- Dev/test: **Dev/Test subscription pricing**, **auto-shutdown**/start schedules, B-series.
- Right-size continuously with **Advisor**; **budgets** with alerts in Cost Management.
- Consider **Azure VMware Solution** only if re-platforming must be avoided (usually costlier per VM).
</details>
