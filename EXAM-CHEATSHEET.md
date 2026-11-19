# AZ-305 Exam Cheatsheet: "If the requirement says X, choose Y"

## Identity and governance
| Requirement | Answer |
|---|---|
| Same policy on many subscriptions | Azure Policy at **management group** |
| Block non-approved regions/SKUs | Azure Policy **Deny** |
| Add or copy tags automatically | Azure Policy **Modify** |
| Prevent deletion | **Resource lock CanNotDelete** |
| Just-in-time admin rights | **PIM** (Entra ID P2) |
| Periodically recertify access | **Access reviews** (P2) |
| Bundle access for partners with expiry | **Entitlement management (access packages)** |
| MFA by location/device/risk | **Conditional Access** (P1; risk needs P2) |
| Sign-in works even if on-prem AD is down | **Password hash sync** |
| On-prem password policy enforced at sign-in, no AD FS | **Pass-through auth** |
| Smartcard/3rd-party federation | **AD FS** |
| Customer sign-in with social accounts | **Azure AD B2C / Entra External ID** |
| Partner employees access your apps | **Entra B2B** |
| Azure resource accesses another without secrets | **Managed identity** (shared across resources → **user-assigned**) |
| GitHub Actions deploys to Azure without secrets | **Workload identity federation** |
| Remote access to on-prem web app without VPN | **Entra application proxy** |
| Store secrets, keys, certs | **Key Vault** (HSM → Premium/Managed HSM) |

## Monitoring
| Requirement | Answer |
|---|---|
| Query logs with KQL, alerts | **Log Analytics workspace** |
| Stream logs to 3rd-party SIEM | **Event Hubs** |
| Cheap long-term log archive | **Storage account** |
| App performance, dependencies, exceptions | **Application Insights** |
| Collect VM guest logs | **Azure Monitor Agent + DCR** |
| Interactive reports | **Workbooks** |
| Analyse huge telemetry volumes fast | **Azure Data Explorer** |
| SIEM/SOAR | **Microsoft Sentinel** |
| Notify/run automation on alert | **Action group** |

## Business continuity
| Requirement | Answer |
|---|---|
| Region failover of VMs, low RPO, orchestrated | **Azure Site Recovery + recovery plan** |
| Backup Azure VMs / Files / SQL in VM | **Azure Backup (Recovery Services vault)** |
| Back up blobs/disks/PostgreSQL/AKS | **Backup vault** |
| On-prem files/folders backup, no server | **MARS agent** |
| Recover overwritten blob | **Versioning** / point-in-time restore |
| WORM compliance | **Immutable blob storage** |
| SQL backups for years | **Long-term retention (up to 10 years)** |
| Survive datacenter failure | **Availability zones** |
| Survive rack failure only | **Availability set** |
| Several SQL DBs fail over together, same connection string | **Auto-failover group** |
| Up to 4 readable SQL secondaries | **Active geo-replication** |
| Read storage during regional outage | **RA-GRS / RA-GZRS** |
| Global multi-write NoSQL | **Cosmos DB multi-region writes** |

## Data
| Requirement | Answer |
|---|---|
| SQL Agent, cross-DB queries, CLR, near-100% compat, PaaS | **SQL Managed Instance** |
| OS access or unsupported features | **SQL Server on VM** |
| Many DBs with variable load | **Elastic pool** |
| Intermittent usage, auto-pause | **Serverless** (SQL DB) |
| Up to 128 TB | **Hyperscale** |
| Low latency + free readable replica | **Business Critical** |
| Reuse licences | **Azure Hybrid Benefit (vCore)** |
| DBAs can't see sensitive columns | **Always Encrypted** |
| Mask data in results | **Dynamic data masking** |
| Per-tenant row filtering | **Row-level security** |
| JSON, global, ms latency | **Cosmos DB** (MongoDB app → Cosmos DB for MongoDB) |
| Big-data files with ACLs | **Data Lake Storage Gen2** |
| Rarely accessed, cheapest | **Archive tier** + lifecycle management |
| Shared SMB/NFS file share | **Azure Files** (HPC/enterprise NFS → **NetApp Files**) |
| Highest disk IOPS | **Ultra Disk** / Premium SSD v2 |
| Temporary scoped storage access | **SAS** (best: user delegation SAS) |
| ETL / orchestrate copies | **Data Factory** (on-prem → self-hosted IR; SSIS → Azure-SSIS IR) |
| Spark, notebooks, ML | **Databricks** |
| Enterprise data warehouse (T-SQL) | **Synapse dedicated SQL pool** |
| Real-time stream with SQL | **Stream Analytics** |

## Compute and apps
| Requirement | Answer |
|---|---|
| Lift-and-shift / full OS control | **VMs** (scale → **VMSS**) |
| HPC/parallel batch | **Azure Batch** (cheap → Spot/low-priority) |
| Web app/API PaaS, slots | **App Service** (isolation → **ASE / Isolated v2**) |
| Simple container, no orchestration | **ACI** |
| Serverless microservices, scale to zero | **Container Apps** |
| Full Kubernetes | **AKS** |
| Event-driven code | **Functions** (no cold start → **Premium**) |
| Low-code workflow with connectors | **Logic Apps** |
| Ordered, transactional, dedup messaging, pub/sub | **Service Bus** (pub/sub → **topics**) |
| Simple, large, cheap queue | **Queue Storage** |
| React to discrete events | **Event Grid** |
| Massive telemetry stream | **Event Hubs** |
| Publish/secure/throttle APIs | **API Management** |
| In-memory cache/sessions | **Azure Cache for Redis** |
| Feature flags, central settings | **App Configuration** |
| Infra as code | **Bicep / ARM / Terraform** + **Azure Pipelines / GitHub Actions** |

## Networking
| Requirement | Answer |
|---|---|
| Private, dedicated, not over the internet | **ExpressRoute** (backup: S2S VPN) |
| Encrypted over internet, cheap | **Site-to-site VPN** |
| Individual laptops | **Point-to-site VPN** |
| Many branches, managed hub | **Virtual WAN** |
| Central shared services | **Hub-spoke** |
| PaaS over private IP from on-prem | **Private endpoint** |
| On-prem ↔ Azure private DNS | **DNS Private Resolver** |
| Global HTTP LB + WAF + CDN | **Front Door** |
| Regional HTTP LB, path routing, WAF | **Application Gateway** |
| Global non-HTTP DNS routing | **Traffic Manager** |
| Regional L4 | **Load Balancer** |
| FQDN filtering, central egress | **Azure Firewall** |
| OWASP protection | **WAF** |
| Volumetric attacks | **DDoS Protection** |
| RDP/SSH without public IP | **Bastion** |
| Static outbound IP / SNAT | **NAT Gateway** |

## Migration
| Requirement | Answer |
|---|---|
| Discover, assess, dependency map | **Azure Migrate appliance** |
| Migrate VMs | **Azure Migrate: Server Migration** |
| VMware as-is in Azure | **Azure VMware Solution** |
| SQL migration online/offline | **DMS / Azure SQL migration extension** (MI near-zero downtime → **MI link / LRS**) |
| Files online | **AzCopy**, **Storage Mover**, **File Sync** |
| Offline ~80 TB / ~770 TB / small | **Data Box / Data Box Heavy / Data Box Disk** |
