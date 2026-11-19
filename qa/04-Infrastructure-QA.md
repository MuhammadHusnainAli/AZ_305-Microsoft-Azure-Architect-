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
