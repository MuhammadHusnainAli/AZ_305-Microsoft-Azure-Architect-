# Q&A: Business Continuity (15–20%)

**Q1.** Define RPO and RTO.
<details><summary>Answer</summary>RPO = max acceptable data loss (time since last good copy). RTO = max acceptable time to restore service.</details>

**Q2.** RPO 15 minutes, RTO 1 hour for 30 Azure VMs if a region fails.
<details><summary>Answer</summary>**Azure Site Recovery** (Azure-to-Azure) with a **recovery plan**.</details>

**Q3.** Test DR without affecting production.
<details><summary>Answer</summary>ASR **test failover** into an isolated VNet.</details>

**Q4.** Restore a single deleted file from last night's VM backup.
<details><summary>Answer</summary>Azure Backup **file-level recovery** (mount recovery point).</details>

**Q5.** Which vault for Azure Blobs and managed disks backup?
<details><summary>Answer</summary>**Backup vault.** (VMs, Azure Files, SQL in VM → Recovery Services vault.)</details>

**Q6.** Back up files on 20 on-prem Windows servers without deploying a backup server.
<details><summary>Answer</summary>**MARS agent** → Recovery Services vault.</details>

**Q7.** Back up on-prem Hyper-V VMs and SQL Server application-consistently.
<details><summary>Answer</summary>**MABS (Microsoft Azure Backup Server)** or DPM.</details>

**Q8.** Ransomware protection: an attacker with admin rights must not delete backups.
<details><summary>Answer</summary>**Immutable vault (locked)** + **soft delete (always-on)** + **multi-user authorization (Resource Guard)**.</details>

**Q9.** Restore VM backups in the paired region during an outage.
<details><summary>Answer</summary>GRS vault with **Cross Region Restore** enabled.</details>

**Q10.** Hourly VM backups.
<details><summary>Answer</summary>Azure Backup **Enhanced policy**.</details>

**Q11.** Users accidentally overwrite blobs; restore previous content.
<details><summary>Answer</summary>Blob **versioning** (and point-in-time restore for container-wide rollback).</details>

**Q12.** Recover a container deleted yesterday.
<details><summary>Answer</summary>**Container soft delete.**</details>

**Q13.** Financial records must be non-modifiable for 7 years (SEC 17a-4).
<details><summary>Answer</summary>**Immutable blob storage** with a locked time-based retention policy.</details>

**Q14.** Azure SQL: restore database to 10:32 yesterday.
<details><summary>Answer</summary>**Point-in-time restore** (within 1–35 day retention).</details>

**Q15.** Keep monthly SQL backups for 10 years.
<details><summary>Answer</summary>**Long-term retention (LTR).**</details>

**Q16.** Restore Azure SQL in another region with RPO ≤ 1 hour at lowest cost.
<details><summary>Answer</summary>**Geo-restore** (GRS backup storage).</details>

**Q17.** SQL RPO 5 seconds, automatic regional failover, no connection string change, several databases.
<details><summary>Answer</summary>**Auto-failover group.**</details>

**Q18.** SQL reporting workloads in 3 other regions with readable copies.
<details><summary>Answer</summary>**Active geo-replication** (up to 4 secondaries).</details>

**Q19.** Composite SLA: App Service 99.95% + SQL 99.99% in series?
<details><summary>Answer</summary>0.9995 × 0.9999 ≈ **99.94%**.</details>

**Q20.** Two independent regions each 99.9%, active-active with perfect failover. Composite?
<details><summary>Answer</summary>1 − (0.001 × 0.001) = **99.9999%** (ignoring the global router's own SLA).</details>

**Q21.** Cheapest way to survive a datacenter failure for VMs.
<details><summary>Answer</summary>Deploy across **availability zones** behind a Standard Load Balancer (99.99%).</details>

**Q22.** Region without availability zones; protect from rack failure.
<details><summary>Answer</summary>**Availability set** (fault + update domains).</details>

**Q23.** App Service must survive zone failure.
<details><summary>Answer</summary>Premium v3 (or Isolated v2) with **zone redundancy** and ≥2/3 instances.</details>

**Q24.** AKS must survive zone failure.
<details><summary>Answer</summary>Node pools spread across **availability zones**, multiple replicas, Standard tier (API server SLA), zone-redundant storage for PVs.</details>

**Q25.** Storage must remain readable during a regional outage, writes can wait.
<details><summary>Answer</summary>**RA-GZRS** (or RA-GRS).</details>

**Q26.** Cosmos DB: 99.999% read **and** write availability.
<details><summary>Answer</summary>**Multi-region writes** across ≥2 regions.</details>

**Q27.** SQL Server on VMs, HA across zones without shared storage.
<details><summary>Answer</summary>**Always On availability groups.**</details>

**Q28.** Failover cluster instance needs shared storage in Azure.
<details><summary>Answer</summary>**Azure shared disks** (Premium/Ultra), Premium file shares, or Storage Spaces Direct.</details>

**Q29.** Global HTTP app across two regions with automatic failover.
<details><summary>Answer</summary>**Azure Front Door** with health probes and priority routing.</details>

**Q30.** Difference between backup and DR?
<details><summary>Answer</summary>Backup protects data (corruption, deletion, ransomware) with point-in-time copies. DR keeps the whole service running elsewhere after a site/region loss.</details>

## More backup and DR
**Q31.** Back up Azure VMs every 4 hours and keep daily points for 30 days, monthly for 7 years.
<details><summary>Answer</summary>Azure Backup **Enhanced policy** with daily, weekly, monthly and yearly retention.</details>

**Q32.** Back up SQL Server running in an Azure VM with 15-minute log backups.
<details><summary>Answer</summary>Azure Backup **SQL Server in Azure VM** workload backup.</details>

**Q33.** Back up SAP HANA in Azure VMs.
<details><summary>Answer</summary>Azure Backup **SAP HANA** (Backint certified).</details>

**Q34.** Back up AKS persistent volumes and cluster state.
<details><summary>Answer</summary>**Azure Backup for AKS** (Backup vault + backup extension).</details>

**Q35.** Back up Azure Database for PostgreSQL beyond 35 days.
<details><summary>Answer</summary>Azure Backup **long-term retention** in a Backup vault.</details>

**Q36.** Monitor and govern backups across 50 subscriptions from one place.
<details><summary>Answer</summary>**Business Continuity Center** / Backup center + Azure Policy to auto-enable backup.</details>

**Q37.** Every new VM tagged `Backup=Gold` must be backed up automatically.
<details><summary>Answer</summary>Azure Policy **"Configure backup on VMs with a given tag"** (DeployIfNotExists).</details>

**Q38.** Recover a VM after ransomware encrypted its disks, with confidence the backup is clean.
<details><summary>Answer</summary>Restore from an earlier recovery point; immutable vault + soft delete stop the attacker from deleting backups. Defender for Cloud helps find the infection point.</details>

**Q39.** On-prem VMware to Azure DR, RPO under 1 minute.
<details><summary>Answer</summary>**Azure Site Recovery** (VMware to Azure, continuous replication).</details>

**Q40.** ASR: a 3-tier app must start DB first, then app, then web, with a DNS update script.
<details><summary>Answer</summary>**Recovery plan** with groups in order + Automation runbook.</details>

**Q41.** Keep the same private IP addresses after failover to Azure.
<details><summary>Answer</summary>Use the same address space in the target VNet; ASR network mapping keeps IPs.</details>

**Q42.** Disaster recovery for Azure Files.
<details><summary>Answer</summary>**GZRS/GRS** with customer-initiated failover, or vaulted backup with cross-region restore.</details>

**Q43.** DR for an App Service web app.
<details><summary>Answer</summary>Deploy to a second region with IaC/CI-CD (active-active or active-passive) behind **Front Door/Traffic Manager**; geo-replicated data store.</details>

**Q44.** DR for Key Vault.
<details><summary>Answer</summary>Key Vault replicates to the paired region automatically (read-only during failover); for full control use a vault per region and replicate secrets via IaC/automation.</details>

**Q45.** What do you test in a DR drill?
<details><summary>Answer</summary>Failover runbooks, RTO/RPO achieved, DNS/connection changes, data consistency, failback, and that people know their roles.</details>

**Q46.** Active-passive vs active-active?
<details><summary>Answer</summary>Active-passive: standby region (warm/cold), cheaper, longer RTO. Active-active: both regions serve traffic, lowest RTO, needs multi-write data or conflict handling, higher cost.</details>

**Q47.** Hot, warm and cold standby?
<details><summary>Answer</summary>Hot: fully running duplicate. Warm: scaled-down running copy that scales up on failover. Cold: infrastructure defined in IaC, deployed only at failover (cheapest, slowest).</details>

## More high availability
**Q48.** Two VMs in one availability set. SLA?
<details><summary>Answer</summary>**99.95%.**</details>

**Q49.** Two VMs across two availability zones. SLA?
<details><summary>Answer</summary>**99.99%.**</details>

**Q50.** Single VM with Premium SSD or Ultra disks. SLA?
<details><summary>Answer</summary>**99.9%.**</details>

**Q51.** Load balancer for zone-redundant VMs?
<details><summary>Answer</summary>**Standard** Load Balancer with a zone-redundant frontend (Basic is retired).</details>

**Q52.** Zone-redundant SQL Database in General Purpose. What changes?
<details><summary>Answer</summary>Compute and storage replicas are spread across zones for a higher SLA (up to 99.995%) with no app change.</details>

**Q53.** Read-only routing to a SQL secondary in the same region.
<details><summary>Answer</summary>**Read scale-out** (Business Critical/Premium/Hyperscale) with `ApplicationIntent=ReadOnly`.</details>

**Q54.** Cosmos DB: what happens with single-write region when the write region fails?
<details><summary>Answer</summary>With **service-managed failover** enabled, another region is promoted to write region automatically.</details>

**Q55.** Make Redis cache survive a zone failure.
<details><summary>Answer</summary>Premium/Enterprise tier with **zone redundancy**.</details>

**Q56.** Service Bus survive a regional outage.
<details><summary>Answer</summary>Premium tier **geo-disaster recovery** (metadata) or **geo-replication** (metadata + data); zone redundancy for zones.</details>

**Q57.** Event Hubs survive a regional outage.
<details><summary>Answer</summary>**Geo-disaster recovery** alias (metadata) or **geo-replication** (Premium/Dedicated).</details>

**Q58.** Storage: avoid single region dependency for a global read-heavy app.
<details><summary>Answer</summary>**RA-GZRS** + read from the secondary endpoint on failure (retry policy with secondary), or Front Door/CDN caching.</details>

**Q59.** What is a single point of failure? Give Azure examples.
<details><summary>Answer</summary>One component whose failure stops the system: one VM, one gateway, one ExpressRoute circuit, one region, one DNS record without failover.</details>

**Q60.** How do health probes improve availability?
<details><summary>Answer</summary>Load balancers/Front Door stop sending traffic to unhealthy instances automatically.</details>
