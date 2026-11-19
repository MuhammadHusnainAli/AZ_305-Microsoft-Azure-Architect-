# Q&A: Data Storage (20–25%)

## Relational
**Q1.** Migrate SQL Server 2016 using SQL Agent jobs, cross-database queries and linked servers. PaaS, minimal changes.
<details><summary>Answer</summary>**Azure SQL Managed Instance.**</details>

**Q2.** App requires SQL Server Reporting Services on the same server and OS-level access.
<details><summary>Answer</summary>**SQL Server on Azure VM.**</details>

**Q3.** 300 small tenant databases, each idle most of the time with random peaks. Minimise cost.
<details><summary>Answer</summary>**Elastic pool.**</details>

**Q4.** A dev database used 2 hours a day; pay nothing for compute when idle.
<details><summary>Answer</summary>**SQL Database serverless** (auto-pause).</details>

**Q5.** Database will grow to 60 TB and needs fast restores regardless of size.
<details><summary>Answer</summary>**Hyperscale** (up to 128 TB, snapshot-based backups).</details>

**Q6.** OLTP needs lowest IO latency and a free readable replica for reports.
<details><summary>Answer</summary>**Business Critical** with read scale-out.</details>

**Q7.** DTU vs vCore — which supports Azure Hybrid Benefit?
<details><summary>Answer</summary>**vCore** only.</details>

**Q8.** Credit-card column must be unreadable by DBAs and the cloud operator.
<details><summary>Answer</summary>**Always Encrypted** (client-side keys).</details>

**Q9.** Support staff see only last 4 digits of phone numbers; no app change.
<details><summary>Answer</summary>**Dynamic data masking.**</details>

**Q10.** Multi-tenant table: each tenant only sees its own rows.
<details><summary>Answer</summary>**Row-level security.**</details>

**Q11.** You must control the TDE key and be able to revoke it.
<details><summary>Answer</summary>**TDE with customer-managed key** in Key Vault (purge protection on).</details>

**Q12.** Detect SQL injection attempts and get vulnerability assessments.
<details><summary>Answer</summary>**Microsoft Defender for SQL.**</details>

**Q13.** Prove that rows have not been tampered with, for auditors.
<details><summary>Answer</summary>**SQL Ledger** tables.</details>

**Q14.** Horizontal scale-out of a single huge multi-tenant database.
<details><summary>Answer</summary>**Sharding** with Elastic Database tools (shard map manager), or Hyperscale / Cosmos DB for PostgreSQL.</details>

**Q15.** Managed PostgreSQL with zone-redundant HA.
<details><summary>Answer</summary>**Azure Database for PostgreSQL Flexible Server**, zone-redundant HA.</details>

## Non-relational
**Q16.** Store product catalogue JSON, single-digit-ms reads worldwide, users in 5 regions.
<details><summary>Answer</summary>**Cosmos DB** (NoSQL API) with multi-region replication.</details>

**Q17.** Existing Cassandra app moves to Azure with minimal code change.
<details><summary>Answer</summary>**Cosmos DB for Apache Cassandra** (or Azure Managed Instance for Apache Cassandra).</details>

**Q18.** Which Cosmos DB consistency is the default and what does it guarantee?
<details><summary>Answer</summary>**Session**: read-your-own-writes within a session.</details>

**Q19.** Users in all regions must always read the latest write, any cost.
<details><summary>Answer</summary>**Strong** consistency (single write region).</details>

**Q20.** Good partition key properties?
<details><summary>Answer</summary>High cardinality, evenly distributes storage and requests, used in most queries (for example `userId`, `tenantId`).</details>

**Q21.** Cosmos DB workload with low, unpredictable traffic.
<details><summary>Answer</summary>**Serverless** capacity mode (or autoscale RU/s if bursts are larger).</details>

**Q22.** Logs kept 7 years, read almost never, cheapest.
<details><summary>Answer</summary>**Blob Archive tier** + lifecycle management policy (and immutability if required).</details>

**Q23.** Move blobs to Cool after 30 days without access and delete after 365 days.
<details><summary>Answer</summary>**Lifecycle management** rule (with last-access-time tracking).</details>

**Q24.** Data lake for Spark with folder-level permissions.
<details><summary>Answer</summary>**ADLS Gen2** (hierarchical namespace + POSIX ACLs).</details>

**Q25.** Lift-and-shift app needs a shared SMB drive with AD permissions.
<details><summary>Answer</summary>**Azure Files** with AD DS (or Entra Kerberos) identity-based auth.</details>

**Q26.** HPC/SAP workload needs sub-millisecond NFS.
<details><summary>Answer</summary>**Azure NetApp Files.**</details>

**Q27.** SAP HANA disk with highest IOPS adjustable without downtime.
<details><summary>Answer</summary>**Ultra Disk** (or Premium SSD v2).</details>

**Q28.** Give a mobile app 1-hour upload access to one container without account keys, revocable.
<details><summary>Answer</summary>**User delegation SAS** (Entra-backed), or service SAS with a **stored access policy** for revocation.</details>

**Q29.** Storage must not be reachable from the internet; VMs and on-prem use a private IP.
<details><summary>Answer</summary>**Private endpoint** + disable public network access + private DNS zone.</details>

**Q30.** Stop anyone using account keys.
<details><summary>Answer</summary>**Disable shared key authorization**; use Entra ID RBAC.</details>

**Q31.** Data must stay in one region but survive a zone outage.
<details><summary>Answer</summary>**ZRS.**</details>

## Data integration
**Q32.** Nightly copy from on-prem Oracle to ADLS.
<details><summary>Answer</summary>**Azure Data Factory** with a **self-hosted integration runtime**.</details>

**Q33.** Run 200 existing SSIS packages in Azure.
<details><summary>Answer</summary>**ADF Azure-SSIS integration runtime.**</details>

**Q34.** Trigger a pipeline when a file lands in Blob storage.
<details><summary>Answer</summary>ADF **storage event trigger.**</details>

**Q35.** Real-time aggregation of IoT readings every 5 minutes, non-overlapping, to Power BI.
<details><summary>Answer</summary>**Stream Analytics** with a **tumbling window**.</details>

**Q36.** Data scientists need notebooks, Spark, MLflow, Delta Lake.
<details><summary>Answer</summary>**Azure Databricks.**</details>

**Q37.** Query Parquet files in the lake with T-SQL, pay per query.
<details><summary>Answer</summary>**Synapse serverless SQL pool.**</details>

**Q38.** Near-real-time analytics on Cosmos DB data without impacting transactions or building ETL.
<details><summary>Answer</summary>**Synapse Link** for Cosmos DB (analytical store) / Fabric mirroring.</details>

**Q39.** Hot vs cold path?
<details><summary>Answer</summary>Hot = real-time stream processing (Stream Analytics); cold = batch over stored historical data (Databricks/Synapse on the lake).</details>
