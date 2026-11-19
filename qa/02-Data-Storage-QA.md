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

## More relational
**Q40.** Which supports native `.bak` restore from on-prem: SQL Database or SQL MI?
<details><summary>Answer</summary>**SQL Managed Instance** (RESTORE FROM URL).</details>

**Q41.** Run one query across 20 sharded Azure SQL databases.
<details><summary>Answer</summary>**Elastic query** (Elastic Database tools).</details>

**Q42.** Schedule index maintenance across many Azure SQL databases (no SQL Agent).
<details><summary>Answer</summary>**Elastic jobs** (or Azure Automation).</details>

**Q43.** Reporting on a General Purpose SQL DB must not affect OLTP.
<details><summary>Answer</summary>Readable **geo-replica**, or move to **Business Critical/Hyperscale** read replicas, or replicate to Synapse/Fabric.</details>

**Q44.** When does an elastic pool save money?
<details><summary>Answer</summary>When the sum of individual database peaks is much higher than the combined peak (databases peak at different times). You pay per pool.</details>

**Q45.** Reduce cost of a 24×7 SQL database for 3 years.
<details><summary>Answer</summary>**Reserved capacity (vCore)** + **Azure Hybrid Benefit**.</details>

**Q46.** Recommended authentication for Azure SQL?
<details><summary>Answer</summary>**Microsoft Entra-only authentication**: managed identities for apps, groups for admins.</details>

**Q47.** Find and label columns with personal data.
<details><summary>Answer</summary>**Data Discovery & Classification** (estate-wide: **Microsoft Purview**).</details>

**Q48.** MySQL app needs reads in another region.
<details><summary>Answer</summary>MySQL Flexible Server **cross-region read replica**.</details>

**Q49.** Distributed PostgreSQL that scales out for multi-tenant SaaS.
<details><summary>Answer</summary>**Azure Cosmos DB for PostgreSQL** (Citus) / PostgreSQL elastic clusters.</details>

**Q50.** Which SQL tier supports In-Memory OLTP and the lowest-latency local storage?
<details><summary>Answer</summary>**Business Critical.**</details>

**Q51.** Hyperscale: how do you get a read-only replica in another region for reporting?
<details><summary>Answer</summary>**Geo-replica** (or named replicas in the same region).</details>

## More non-relational
**Q52.** Cosmos DB hot partition: one tenant gets 80% of traffic.
<details><summary>Answer</summary>Better partition key, **hierarchical/synthetic partition key** (tenantId + userId), or a dedicated container for big tenants.</details>

**Q53.** Cosmos DB returns HTTP 429 under load.
<details><summary>Answer</summary>Requests exceed **RU/s**: raise RU/s or use **autoscale**, optimise queries and indexing, retry with backoff.</details>

**Q54.** Lower the RU cost of writes for documents with many properties.
<details><summary>Answer</summary>Custom **indexing policy** that excludes unused paths.</details>

**Q55.** Delete documents automatically after 30 days.
<details><summary>Answer</summary>Cosmos DB **TTL**.</details>

**Q56.** React to every insert or update in a Cosmos DB container.
<details><summary>Answer</summary>**Change feed** (Azure Functions Cosmos DB trigger).</details>

**Q57.** Restore a Cosmos DB container to its state 2 hours ago.
<details><summary>Answer</summary>**Continuous backup** with **point-in-time restore**.</details>

**Q58.** Social network friends-of-friends queries.
<details><summary>Answer</summary>**Cosmos DB for Apache Gremlin** (graph).</details>

**Q59.** Cheap hosting for a static website.
<details><summary>Answer</summary>Blob **static website** or **Azure Static Web Apps**, plus Front Door/CDN.</details>

**Q60.** Asynchronously copy selected containers to an account in another region.
<details><summary>Answer</summary>**Object replication.**</details>

**Q61.** Scan uploaded blobs for malware.
<details><summary>Answer</summary>**Defender for Storage** malware scanning.</details>

**Q62.** VM disks, temp disk and caches must be encrypted with your own key.
<details><summary>Answer</summary>**Encryption at host** + **customer-managed keys** (Disk Encryption Set).</details>

**Q63.** Disks need short bursts of extra performance at boot time.
<details><summary>Answer</summary>Premium SSD **disk bursting**.</details>

**Q64.** An archived blob is needed urgently.
<details><summary>Answer</summary>Rehydrate with **High priority** (often under 1 hour for small blobs).</details>

**Q65.** Several branch offices need fast local access to the same share.
<details><summary>Answer</summary>**Azure File Sync** with a server endpoint at each branch.</details>

**Q66.** A storage account is hitting its request-rate limit.
<details><summary>Answer</summary>Spread data across **multiple accounts**, use **premium** accounts, cache with CDN.</details>

**Q67.** Which redundancy options support customer-initiated account failover?
<details><summary>Answer</summary>**GRS, RA-GRS, GZRS, RA-GZRS.**</details>

**Q68.** Data must never leave the country, but must survive a zone outage.
<details><summary>Answer</summary>**ZRS** (GRS may replicate to a paired region; check the pair's location).</details>

## More data integration
**Q69.** ADF should copy only rows changed since the last run.
<details><summary>Answer</summary>**Incremental load** with a watermark column, or **change data capture**.</details>

**Q70.** ADF must reach a data source in a private Azure VNet without your own VMs.
<details><summary>Answer</summary>**Managed VNet integration runtime** with **managed private endpoints**.</details>

**Q71.** Where does ADF keep connection secrets?
<details><summary>Answer</summary>**Key Vault linked service** accessed with ADF's managed identity.</details>

**Q72.** How should you organise data in a lake?
<details><summary>Answer</summary>**Medallion**: bronze (raw) → silver (cleaned) → gold (business-ready), stored as **Delta/Parquet**.</details>

**Q73.** Catalogue, classify and see the lineage of data across Azure and on-prem.
<details><summary>Answer</summary>**Microsoft Purview.**</details>

**Q74.** Count events in 10-minute windows produced every 5 minutes (overlapping).
<details><summary>Answer</summary>Stream Analytics **hopping window** (size 10, hop 5).</details>

**Q75.** Group clicks into sessions separated by 30 minutes of inactivity.
<details><summary>Answer</summary>**Session window.**</details>

**Q76.** One SaaS platform for ingestion, lakehouse, warehouse, real-time and Power BI.
<details><summary>Answer</summary>**Microsoft Fabric** (OneLake).</details>

**Q77.** Join a real-time stream with a slowly changing product list.
<details><summary>Answer</summary>Stream Analytics **reference data** input (Blob/SQL).</details>
