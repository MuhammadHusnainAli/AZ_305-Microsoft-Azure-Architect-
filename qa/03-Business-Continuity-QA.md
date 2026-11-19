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
