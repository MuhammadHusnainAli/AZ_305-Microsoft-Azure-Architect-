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
