# Q&A: Infrastructure (30–35%)

## Compute
**Q1.** Legacy Windows app with a custom driver must move to Azure this month.
<details><summary>Answer</summary>**Azure VM** (rehost).</details>

**Q2.** Web tier must scale from 2 to 50 identical VMs on CPU.
<details><summary>Answer</summary>**VM Scale Set** with autoscale rules.</details>

**Q3.** Licensing requires dedicated physical servers.
<details><summary>Answer</summary>**Azure Dedicated Host.**</details>

**Q4.** Render 100,000 frames overnight, cheapest.
<details><summary>Answer</summary>**Azure Batch** with **Spot/low-priority** nodes.</details>

**Q5.** Tightly-coupled MPI simulation needs low-latency interconnect.
<details><summary>Answer</summary>Azure Batch or VMSS with **H-series (InfiniBand)** VMs, (or Azure CycleCloud).</details>

**Q6.** .NET web API, zero-downtime releases, autoscale, no infra management.
<details><summary>Answer</summary>**App Service** Standard+ with **deployment slots**.</details>

**Q7.** Web app must be fully network-isolated on single-tenant hardware.
<details><summary>Answer</summary>**App Service Environment v3 (Isolated v2).**</details>

**Q8.** Run a container once a day for 10 minutes, simplest.
<details><summary>Answer</summary>**Azure Container Instances** (or Container Apps job).</details>

**Q9.** 20 microservices, scale to zero, event-driven scaling from Service Bus, no Kubernetes ops.
<details><summary>Answer</summary>**Azure Container Apps** (KEDA scalers).</details>

**Q10.** Team needs custom Kubernetes operators, service mesh and full K8s API.
<details><summary>Answer</summary>**AKS.**</details>

**Q11.** AKS must burst quickly without adding nodes.
<details><summary>Answer</summary>**Virtual nodes** (ACI).</details>

**Q12.** Function must not have cold starts and must access a VNet.
<details><summary>Answer</summary>Functions **Premium** (or Flex Consumption with always-ready instances).</details>

**Q13.** Orchestrate a long-running workflow in code with fan-out/fan-in.
<details><summary>Answer</summary>**Durable Functions.**</details>

**Q14.** When an invoice email arrives, extract the PDF, save to SharePoint and post to Teams; business users maintain it.
<details><summary>Answer</summary>**Logic Apps.**</details>

**Q15.** Remote employees need a full Windows desktop with central management, multi-session.
<details><summary>Answer</summary>**Azure Virtual Desktop** (Windows 11 Enterprise multi-session).</details>

## Application architecture
**Q16.** Order messages must be processed FIFO per customer, no duplicates.
<details><summary>Answer</summary>**Service Bus** with **sessions** (SessionId = customerId) + **duplicate detection**.</details>

**Q17.** One "OrderPlaced" message must reach billing, shipping and email, each filtering what it needs.
<details><summary>Answer</summary>**Service Bus topic** with subscriptions and filters.</details>

**Q18.** Simple work queue, >80 GB backlog, cheapest.
<details><summary>Answer</summary>**Azure Queue Storage.**</details>

**Q19.** Process images whenever they are uploaded to Blob storage.
<details><summary>Answer</summary>**Event Grid** (BlobCreated) → Function.</details>

**Q20.** Ingest 2 million events/s from cars, replay last 7 days, Kafka clients.
<details><summary>Answer</summary>**Event Hubs** (Kafka endpoint, retention, partitions).</details>

**Q21.** Messages that fail processing 10 times go where?
<details><summary>Answer</summary>Service Bus **dead-letter queue**.</details>

**Q22.** Expose internal APIs to partners with keys, rate limits and documentation.
<details><summary>Answer</summary>**API Management** (products, subscriptions, rate-limit policy, developer portal).</details>

**Q23.** APIM must run in VNet, multi-region, with zone redundancy.
<details><summary>Answer</summary>**APIM Premium** tier.</details>

**Q24.** Manage APIs hosted on-prem and in AWS through one control plane.
<details><summary>Answer</summary>APIM **self-hosted gateway.**</details>

**Q25.** Product catalogue read thousands of times per second; reduce SQL load.
<details><summary>Answer</summary>**Azure Cache for Redis** with cache-aside.</details>

**Q26.** Static images load slowly for global users.
<details><summary>Answer</summary>**Azure Front Door / CDN** caching.</details>

**Q27.** Toggle a new checkout feature for 10% of users without redeploying.
<details><summary>Answer</summary>**Azure App Configuration feature flags.**</details>

**Q28.** Deploy identical environments repeatably, readable IaC, Azure-native.
<details><summary>Answer</summary>**Bicep** (with template specs / deployment stacks) via Azure Pipelines or GitHub Actions.</details>

## Networking
**Q29.** 10 Gbps private, predictable connection; must not traverse the internet.
<details><summary>Answer</summary>**ExpressRoute** (private peering).</details>

**Q30.** ExpressRoute needs a backup path.
<details><summary>Answer</summary>**Site-to-site VPN** as failover (or a second circuit in another peering location).</details>

**Q31.** Connect 300 branch offices and 4 regions with any-to-any routing, minimal management.
<details><summary>Answer</summary>**Azure Virtual WAN.**</details>

**Q32.** Spoke A must reach spoke B in hub-spoke.
<details><summary>Answer</summary>Peering is **non-transitive**: route via the hub **Azure Firewall/NVA** with **UDRs**, or peer directly, or use Virtual WAN / AVNM.</details>

**Q33.** Service endpoint vs private endpoint?
<details><summary>Answer</summary>Service endpoint: VNet identity to a PaaS public endpoint, free, not reachable from on-prem. **Private endpoint**: private IP in VNet, reachable from on-prem/peered VNets, prevents exfiltration, needs DNS.</details>

**Q34.** On-prem DNS must resolve Azure private endpoint names without DNS VMs.
<details><summary>Answer</summary>**Azure DNS Private Resolver** (inbound endpoint) + conditional forwarder.</details>

**Q35.** Global website with WAF, SSL offload, caching and instant failover.
<details><summary>Answer</summary>**Azure Front Door Premium** (WAF).</details>

**Q36.** Regional web app: route `/api/*` to AKS and `/*` to VMs, WAF.
<details><summary>Answer</summary>**Application Gateway (WAF v2)**, path-based routing.</details>

**Q37.** Global failover for a TCP game server.
<details><summary>Answer</summary>**Traffic Manager** (DNS, priority/performance), or cross-region Load Balancer.</details>

**Q38.** Users in Germany must reach only the EU deployment.
<details><summary>Answer</summary>Traffic Manager **geographic routing** (or Front Door rules).</details>

**Q39.** Allow outbound only to `*.github.com` from all spokes.
<details><summary>Answer</summary>**Azure Firewall** application rule (FQDN) with forced routing via UDR.</details>

**Q40.** Inspect TLS traffic and use IDPS.
<details><summary>Answer</summary>**Azure Firewall Premium.**</details>

**Q41.** RDP to VMs with no public IPs.
<details><summary>Answer</summary>**Azure Bastion.**</details>

**Q42.** Outbound connections fail with SNAT exhaustion.
<details><summary>Answer</summary>**NAT Gateway** on the subnet.</details>

**Q43.** Group web VMs so NSG rules reference "WebServers" instead of IPs.
<details><summary>Answer</summary>**Application security groups.**</details>

**Q44.** Lowest latency between tiers of VMs.
<details><summary>Answer</summary>**Proximity placement group** + accelerated networking.</details>

**Q45.** Protect public IPs from volumetric attacks with cost protection.
<details><summary>Answer</summary>**Azure DDoS Network Protection.**</details>

## Migration
**Q46.** Discover 800 VMware VMs agentlessly and find dependencies.
<details><summary>Answer</summary>**Azure Migrate appliance** + agentless dependency analysis.</details>

**Q47.** Build a cost case to leadership for moving to Azure.
<details><summary>Answer</summary>**Azure Migrate business case** (and TCO calculator).</details>

**Q48.** Migrate VMware without re-platforming or retraining staff.
<details><summary>Answer</summary>**Azure VMware Solution** (HCX).</details>

**Q49.** SQL Server → SQL MI with minimal downtime.
<details><summary>Answer</summary>**Managed Instance link** or **DMS online** / Log Replay Service.</details>

**Q50.** 60 TB of files, 50 Mbps internet, need it in 2 weeks.
<details><summary>Answer</summary>**Azure Data Box** (offline).</details>

**Q51.** Migrate NFS shares to Azure Files with a managed, orchestrated service.
<details><summary>Answer</summary>**Azure Storage Mover.**</details>

**Q52.** Which CAF phase prepares landing zones before migration?
<details><summary>Answer</summary>**Ready.**</details>

## More compute
**Q53.** VM family for an in-memory database needing lots of RAM.
<details><summary>Answer</summary>**E-series** (or M-series for very large memory).</details>

**Q54.** VM family for GPU training or visualisation.
<details><summary>Answer</summary>**N-series** (NC/ND for compute/AI, NV for visualisation).</details>

**Q55.** Low-cost VM for a dev server with low average CPU and occasional spikes.
<details><summary>Answer</summary>**B-series** (burstable).</details>

**Q56.** Keep a golden VM image versioned and replicated to 4 regions.
<details><summary>Answer</summary>**Azure Compute Gallery** (image builder optional).</details>

**Q57.** Patch 1,000 Windows and Linux VMs on a schedule, including Arc servers.
<details><summary>Answer</summary>**Azure Update Manager** with maintenance configurations.</details>

**Q58.** Open RDP only for 3 hours when an admin requests it.
<details><summary>Answer</summary>Defender for Cloud **just-in-time VM access** (or Bastion).</details>

**Q59.** VMSS: replace instances gradually when the image changes.
<details><summary>Answer</summary>**Rolling upgrade policy** (or automatic OS image upgrades).</details>

**Q60.** Run Windows containers and Linux containers in one AKS cluster.
<details><summary>Answer</summary>Separate **Windows and Linux node pools**.</details>

**Q61.** Reduce AKS cost for fault-tolerant batch pods.
<details><summary>Answer</summary>**Spot node pool** with taints/tolerations, cluster autoscaler.</details>

**Q62.** AKS pods need IPs directly from the VNet.
<details><summary>Answer</summary>**Azure CNI** (or Azure CNI Overlay to save IPs).</details>

**Q63.** Store and scan container images, replicate to 3 regions.
<details><summary>Answer</summary>**Azure Container Registry Premium** (geo-replication) + Defender for Containers.</details>

**Q64.** Run a container job every hour that finishes in 5 minutes.
<details><summary>Answer</summary>**Container Apps jobs** (scheduled) or ACI with Logic Apps.</details>

**Q65.** App Service: scale out vs scale up?
<details><summary>Answer</summary>Scale out = more instances (autoscale). Scale up = bigger plan (more CPU/RAM, features like slots/VNet).</details>

**Q66.** App Service needs to call a SQL MI in a private VNet.
<details><summary>Answer</summary>**Regional VNet integration** (outbound).</details>

**Q67.** App Service must only be reachable from the corporate network.
<details><summary>Answer</summary>**Private endpoint** (inbound) and disable public access, or access restrictions.</details>

**Q68.** Host a containerised web app with minimal ops and built-in slots.
<details><summary>Answer</summary>**Web App for Containers** (App Service).</details>

**Q69.** Run Spring Boot / Java microservices with managed platform.
<details><summary>Answer</summary>**Azure Container Apps** (Azure Spring Apps is being retired) or App Service.</details>

**Q70.** Function triggered by a timer every night.
<details><summary>Answer</summary>Functions **timer trigger** (CRON expression).</details>

**Q71.** Functions Consumption plan maximum execution time?
<details><summary>Answer</summary>Default 5 minutes, max 10 minutes. For longer runs use Premium/Flex/Dedicated or Durable Functions.</details>

**Q72.** Integrate SAP, Salesforce and on-prem SQL with B2B/EDI and VNet isolation.
<details><summary>Answer</summary>**Logic Apps Standard** + integration account (or Azure Integration Services).</details>

**Q73.** Run HPC with a scheduler like Slurm.
<details><summary>Answer</summary>**Azure CycleCloud** (or Azure Batch for cloud-native batch).</details>

## More application architecture
**Q74.** Large messages (50 MB images) through a queue.
<details><summary>Answer</summary>**Claim-check pattern**: store the payload in Blob, send a reference in the message (or Service Bus Premium large messages).</details>

**Q75.** Messages must be processed at a scheduled time tomorrow.
<details><summary>Answer</summary>Service Bus **scheduled messages**.</details>

**Q76.** Several services must commit sending multiple messages atomically.
<details><summary>Answer</summary>Service Bus **transactions**.</details>

**Q77.** Event Hubs: two independent apps read the same stream at their own pace.
<details><summary>Answer</summary>Separate **consumer groups**.</details>

**Q78.** Event Hubs: archive every event to the lake without code.
<details><summary>Answer</summary>**Event Hubs Capture** to Blob/ADLS (Avro/Parquet).</details>

**Q79.** Event Grid vs Service Bus for "OrderPlaced"?
<details><summary>Answer</summary>Event Grid for lightweight reactive notifications (push, fan-out). Service Bus when the business must reliably process every order with ordering/transactions.</details>

**Q80.** IoT devices publish over MQTT with millions of connections.
<details><summary>Answer</summary>**Event Grid MQTT broker** (namespaces) or **IoT Hub**.</details>

**Q81.** Real-time push notifications to web browsers (chat, dashboards).
<details><summary>Answer</summary>**Azure SignalR Service** / **Web PubSub**.</details>

**Q82.** Store user session state for a web farm.
<details><summary>Answer</summary>**Azure Cache for Redis** (session provider).</details>

**Q83.** Redis data must survive restarts.
<details><summary>Answer</summary>Premium tier **persistence** (RDB/AOF).</details>

**Q84.** APIM: validate JWTs from Entra ID before reaching the backend.
<details><summary>Answer</summary>**validate-jwt** (or validate-azure-ad-token) policy.</details>

**Q85.** APIM: limit each partner to 1,000 calls per hour.
<details><summary>Answer</summary>**rate-limit-by-key / quota-by-key** policy per subscription.</details>

**Q86.** Cheapest APIM for a few serverless APIs with spiky traffic.
<details><summary>Answer</summary>**Consumption** tier.</details>

**Q87.** Govern and catalogue all APIs across the company (not a gateway).
<details><summary>Answer</summary>**Azure API Center.**</details>

**Q88.** Change app settings without restarting 50 instances.
<details><summary>Answer</summary>**App Configuration** with dynamic refresh (sentinel key).</details>

**Q89.** Prevent configuration drift in Azure resources.
<details><summary>Answer</summary>IaC as the only change path (CI/CD), Azure Policy, **deployment stacks** with deny settings, what-if in pipelines.</details>

**Q90.** Preview infrastructure changes before deploying a Bicep file.
<details><summary>Answer</summary>**what-if** operation.</details>

**Q91.** Approval gate before production deployment.
<details><summary>Answer</summary>Azure Pipelines **environments with approvals/checks** or GitHub **environment protection rules**.</details>

## More networking
**Q92.** How many VNets can a single VNet peer with directly, and is peering transitive?
<details><summary>Answer</summary>Up to 500 peerings by default (higher with AVNM); peering is **not transitive**.</details>

**Q93.** Spokes must use the hub's VPN gateway to reach on-prem.
<details><summary>Answer</summary>Peering with **Allow gateway transit** (hub) and **Use remote gateways** (spokes).</details>

**Q94.** Force all internet-bound traffic from spokes through Azure Firewall.
<details><summary>Answer</summary>**UDR** 0.0.0.0/0 → firewall private IP on each spoke subnet.</details>

**Q95.** Force internet traffic back on-prem for inspection.
<details><summary>Answer</summary>**Forced tunnelling** (default route advertised via BGP/VPN).</details>

**Q96.** Point-to-site VPN users authenticate with Entra ID and MFA.
<details><summary>Answer</summary>P2S with **OpenVPN** protocol + **Microsoft Entra ID authentication**.</details>

**Q97.** VPN gateway must survive a zone failure.
<details><summary>Answer</summary>**Zone-redundant SKU (VpnGwXAZ)**, active-active.</details>

**Q98.** ExpressRoute: access Microsoft 365 and Azure public services privately.
<details><summary>Answer</summary>**Microsoft peering** (with route filters).</details>

**Q99.** Connect two on-prem sites through Microsoft's backbone using existing ExpressRoute circuits.
<details><summary>Answer</summary>**ExpressRoute Global Reach.**</details>

**Q100.** Private DNS for VMs in multiple VNets with auto-registration.
<details><summary>Answer</summary>**Azure Private DNS zone** linked to VNets with auto-registration.</details>

**Q101.** Expose your own SaaS service privately to customers' VNets.
<details><summary>Answer</summary>**Private Link service** behind a Standard Load Balancer.</details>

**Q102.** Restrict a storage account to one subnet without private IPs, at no cost.
<details><summary>Answer</summary>**Service endpoint** + storage firewall VNet rule (plus service endpoint policy to prevent exfiltration).</details>

**Q103.** Application Gateway must terminate TLS and re-encrypt to backend.
<details><summary>Answer</summary>**End-to-end TLS** on Application Gateway.</details>

**Q104.** Many websites with different hostnames on one Application Gateway.
<details><summary>Answer</summary>**Multi-site listeners** (host-based routing).</details>

**Q105.** Front Door must reach an App Service that has no public access.
<details><summary>Answer</summary>Front Door **Premium** with **Private Link origin**.</details>

**Q106.** Allow traffic to App Service only from Front Door.
<details><summary>Answer</summary>Access restriction with service tag **AzureFrontDoor.Backend** + check the `X-Azure-FDID` header (or Private Link).</details>

**Q107.** Block traffic from specific countries at the edge.
<details><summary>Answer</summary>**WAF geo-filtering** custom rule on Front Door.</details>

**Q108.** Central NSG/admin rules across 200 VNets.
<details><summary>Answer</summary>**Azure Virtual Network Manager** security admin rules.</details>

**Q109.** NVA high availability.
<details><summary>Answer</summary>Two+ NVAs behind a **Standard internal Load Balancer** with **HA ports** (or Gateway Load Balancer).</details>

**Q110.** Measure latency and packet loss between on-prem and Azure continuously.
<details><summary>Answer</summary>Network Watcher **Connection Monitor.**</details>

## More migration
**Q111.** Rehost 500 Hyper-V VMs with minimal downtime.
<details><summary>Answer</summary>**Azure Migrate: Server Migration** (replication + test migration + cutover).</details>

**Q112.** What does an Azure Migrate assessment give you?
<details><summary>Answer</summary>Readiness, right-sized Azure targets (performance-based or as-is), monthly cost estimates, and confidence rating.</details>

**Q113.** Assessment confidence rating is low. Why?
<details><summary>Answer</summary>Not enough performance data collected (collect for longer, e.g. a month) or missing counters.</details>

**Q114.** Migrate Oracle to Azure.
<details><summary>Answer</summary>Rehost on Azure VMs, **Oracle Database@Azure**, or convert to PostgreSQL (with migration tools).</details>

**Q115.** Migrate MongoDB to Azure with minimal downtime.
<details><summary>Answer</summary>**Azure DMS (online)** to **Cosmos DB for MongoDB** (vCore or RU).</details>

**Q116.** Migrate ASP.NET apps from IIS servers at scale.
<details><summary>Answer</summary>Azure Migrate **web app assessment and migration** to App Service (or AKS containers).</details>

**Q117.** Copy 10 TB from AWS S3 to Azure Blob.
<details><summary>Answer</summary>**AzCopy** (S3 to Blob directly) or **Data Factory**.</details>

**Q118.** Order of a typical migration wave?
<details><summary>Answer</summary>Landing zone ready → assess & group by dependency → pilot migration → test → cut over → decommission → optimise.</details>
