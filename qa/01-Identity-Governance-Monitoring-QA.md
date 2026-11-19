# Q&A: Identity, Governance and Monitoring (25–30%)

## Governance
**Q1.** You have 60 subscriptions across 3 business units. Each unit needs its own policies, and all must deny resources outside the EU. Design the hierarchy.
<details><summary>Answer</summary>Root MG → one MG per business unit → subscriptions. Assign the "Allowed locations = EU" policy at the **root/top MG** (inherited by all), and assign unit-specific policies at each **business-unit MG**.</details>

**Q2.** Every resource must have a `CostCenter` tag equal to its resource group's tag, including existing resources.
<details><summary>Answer</summary>Azure Policy "Inherit a tag from the resource group" (**Modify** effect) plus a **remediation task** for existing resources. Tags are not inherited by default.</details>

**Q3.** Difference between Azure Policy and RBAC?
<details><summary>Answer</summary>Policy controls **what** resource properties are allowed (compliance). RBAC controls **who** can perform actions. You usually need both.</details>

**Q4.** Production resource group must never be deleted, even by Owners, by accident.
<details><summary>Answer</summary>A **CanNotDelete resource lock** on the RG. Locks apply to everyone, including Owners, until removed.</details>

**Q5.** Which policy effect deploys the Log Analytics agent/diagnostic setting when missing?
<details><summary>Answer</summary>**DeployIfNotExists** (with a managed identity and remediation task).</details>

**Q6.** You need to group 20 policies for ISO 27001 and track compliance as one score.
<details><summary>Answer</summary>A policy **initiative** (policy set), assigned at the MG; view in Defender for Cloud regulatory compliance.</details>

**Q7.** Maximum management group depth?
<details><summary>Answer</summary>**6 levels**, not counting the root or the subscription level.</details>

**Q8.** When should you create a new subscription instead of a new resource group?
<details><summary>Answer</summary>Separate billing/cost ownership, different compliance boundary, subscription limits/quotas reached, separate environments (prod/dev), or network/admin isolation.</details>

## Identity
**Q9.** Users must sign in to Microsoft 365 even if the on-prem datacenter is offline. Least admin effort.
<details><summary>Answer</summary>**Password hash synchronization** (+ Seamless SSO).</details>

**Q10.** Company policy: sign-ins must respect AD logon hours and account lockout instantly. No AD FS.
<details><summary>Answer</summary>**Pass-through authentication.** Enable PHS as a backup.</details>

**Q11.** Users must log in with smartcards via a third-party IdP.
<details><summary>Answer</summary>**Federation (AD FS or a third-party federation provider).**</details>

**Q12.** Require MFA only for sign-ins from outside the corporate IP range. Which licence?
<details><summary>Answer</summary>**Conditional Access** with a named location — **Entra ID P1**.</details>

**Q13.** Force password change when a user's credentials are found leaked on the dark web.
<details><summary>Answer</summary>**Identity Protection user-risk policy** (risk-based Conditional Access) — **Entra ID P2**.</details>

**Q14.** Admins must request Global Administrator for 2 hours with approval and justification.
<details><summary>Answer</summary>**Privileged Identity Management (PIM)**, P2.</details>

**Q15.** Every quarter, managers must confirm guests still need access; unanswered = removed.
<details><summary>Answer</summary>**Access reviews** with auto-apply and "remove access if reviewers don't respond".</details>

**Q16.** Partner company employees need access to a SharePoint site and an app for 90 days with approval.
<details><summary>Answer</summary>**Entra B2B** + **entitlement management access package** with expiration and approval.</details>

**Q17.** Public retail app: customers sign up with Google, Facebook or email, custom branded pages.
<details><summary>Answer</summary>**Azure AD B2C / Microsoft Entra External ID** (customer tenant).</details>

**Q18.** 50 VMs must read the same Key Vault secret; identity must outlive individual VMs.
<details><summary>Answer</summary>**User-assigned managed identity** shared by all VMs, with the Key Vault Secrets User role.</details>

**Q19.** An App Service must access Azure SQL without any password in config.
<details><summary>Answer</summary>Enable a **managed identity**, create an Entra user for it in SQL, use Entra auth.</details>

**Q20.** GitHub Actions must deploy to Azure without storing a client secret.
<details><summary>Answer</summary>**Workload identity federation** (OIDC federated credential on an app registration or user-assigned managed identity).</details>

**Q21.** Remote employees need access to an internal on-prem web app using Entra MFA, no VPN, no inbound ports.
<details><summary>Answer</summary>**Microsoft Entra application proxy** (connector makes outbound connections) — or Entra Private Access.</details>

**Q22.** Lift-and-shift app needs LDAP and Kerberos in Azure, no domain controllers to manage.
<details><summary>Answer</summary>**Microsoft Entra Domain Services.**</details>

## Secrets and keys
**Q23.** Encryption keys must be in a FIPS 140-3 Level 3 single-tenant HSM you control.
<details><summary>Answer</summary>**Azure Key Vault Managed HSM.**</details>

**Q24.** A deleted Key Vault must be recoverable and nobody may purge it during retention.
<details><summary>Answer</summary>**Soft delete + purge protection.**</details>

**Q25.** How many Key Vaults should you create?
<details><summary>Answer</summary>One **per application, per environment, per region** — limits blast radius and throttling.</details>

**Q26.** Certificates must renew automatically.
<details><summary>Answer</summary>Key Vault **certificates** with an integrated CA (DigiCert/GlobalSign) and auto-renewal policy.</details>

## Monitoring
**Q27.** Security team uses Splunk. Send Entra sign-in logs and Activity logs to it.
<details><summary>Answer</summary>Diagnostic settings → **Event Hubs** → Splunk.</details>

**Q28.** Keep Activity log for 7 years at lowest cost.
<details><summary>Answer</summary>Diagnostic setting → **Storage account** (archive tier / immutability if needed).</details>

**Q29.** Query logs from all subscriptions with KQL and create alerts.
<details><summary>Answer</summary>Central **Log Analytics workspace**, diagnostic settings via Azure Policy (DeployIfNotExists).</details>

**Q30.** Data residency: EU logs must stay in the EU, US logs in the US.
<details><summary>Answer</summary>**Separate Log Analytics workspaces per region/geography.**</details>

**Q31.** App teams should only see logs for resources they have RBAC on, in a shared workspace.
<details><summary>Answer</summary>**Resource-context access mode** (access control mode: use resource or workspace permissions).</details>

**Q32.** Collect Windows Security events from 500 VMs, filter only certain event IDs.
<details><summary>Answer</summary>**Azure Monitor Agent** + **Data Collection Rule** with XPath filter.</details>

**Q33.** Detect slow dependencies and exceptions in a web app; see an application map.
<details><summary>Answer</summary>**Application Insights.**</details>

**Q34.** Notify on-call via SMS and run a runbook when CPU > 90%.
<details><summary>Answer</summary>**Metric alert** + **action group** (SMS + Automation runbook).</details>

**Q35.** Suppress alerts during a Saturday maintenance window.
<details><summary>Answer</summary>**Alert processing rule** with a schedule.</details>

**Q36.** Be notified when Azure has an outage affecting your region's services.
<details><summary>Answer</summary>**Azure Service Health alert.**</details>

**Q37.** Analyse billions of IoT telemetry records interactively over years.
<details><summary>Answer</summary>**Azure Data Explorer** (KQL, time-series).</details>

## More governance
**Q38.** A team may create VMs only in approved sizes (B and D series).
<details><summary>Answer</summary>Azure Policy **Allowed virtual machine size SKUs** (Deny) at the subscription or MG.</details>

**Q39.** How do you roll out a new policy safely?
<details><summary>Answer</summary>Assign with **Audit** effect (or enforcement mode **DoNotEnforce**), review compliance, then switch to **Deny**. Use **exemptions** for approved exceptions.</details>

**Q40.** One resource must be excluded from a Deny policy for 30 days.
<details><summary>Answer</summary>**Policy exemption** (category Waiver) with an expiration date.</details>

**Q41.** Block deletion of resources tagged `Protected=true` without using a lock.
<details><summary>Answer</summary>Azure Policy **DenyAction** effect on the delete action.</details>

**Q42.** Manage resources in 30 customer tenants from your MSP tenant.
<details><summary>Answer</summary>**Azure Lighthouse** (delegated resource management).</details>

**Q43.** Query the configuration of thousands of resources across subscriptions quickly.
<details><summary>Answer</summary>**Azure Resource Graph** (KQL).</details>

**Q44.** Built-in role that lets a team manage only policy assignments.
<details><summary>Answer</summary>**Resource Policy Contributor.**</details>

**Q45.** Users may restart VMs but not delete them.
<details><summary>Answer</summary>Custom RBAC role with `Microsoft.Compute/virtualMachines/restart/action` (+ read), no delete action.</details>

**Q46.** Control plane vs data plane permissions?
<details><summary>Answer</summary>Control plane (`Actions`) manages the resource through ARM, e.g. create a storage account. Data plane (`DataActions`) accesses the data, e.g. read a blob. Contributor does not let you read blobs with Entra auth; you need **Storage Blob Data Reader**.</details>

**Q47.** Show cost per department across shared subscriptions.
<details><summary>Answer</summary>**Tags** (Department/CostCenter) enforced by Policy + **Cost Management** grouped by tag; or separate subscriptions per department.</details>

**Q48.** Find who changed an NSG rule yesterday.
<details><summary>Answer</summary>**Activity log** (control-plane operations) or Change Analysis.</details>

**Q49.** How do you define and enforce naming conventions?
<details><summary>Answer</summary>Use the CAF naming standard (resource type abbreviation, workload, environment, region, instance). Enforce it with **Azure Policy** pattern rules or IaC modules.</details>

## More identity
**Q50.** Nobody may register a device without MFA.
<details><summary>Answer</summary>Conditional Access on the user action **Register or join devices** → **Require MFA**.</details>

**Q51.** Block legacy authentication (POP, IMAP, basic auth) tenant-wide.
<details><summary>Answer</summary>Conditional Access: client apps = **Exchange ActiveSync / Other clients** → **Block**.</details>

**Q52.** Admins must use phishing-resistant MFA.
<details><summary>Answer</summary>Conditional Access **authentication strength** = phishing-resistant (FIDO2, Windows Hello for Business, certificate-based auth).</details>

**Q53.** Small tenant without P1 licences wants baseline MFA for everyone.
<details><summary>Answer</summary>**Security defaults.**</details>

**Q54.** Users reset their own passwords and the change must reach on-prem AD.
<details><summary>Answer</summary>**Self-service password reset** with **password writeback** (P1).</details>

**Q55.** Group membership should follow the department attribute automatically.
<details><summary>Answer</summary>**Dynamic groups** (P1).</details>

**Q56.** New hires get access on day one; leavers lose it automatically.
<details><summary>Answer</summary>HR-driven provisioning + **lifecycle workflows** + **access packages** (Entra ID Governance).</details>

**Q57.** Several AD forests with no network connection between them must sync to one tenant.
<details><summary>Answer</summary>**Microsoft Entra Cloud Sync** (lightweight agents in each forest).</details>

**Q58.** Emergency access if MFA or Conditional Access is broken.
<details><summary>Answer</summary>Two cloud-only **break-glass accounts**, excluded from CA, strong credentials (FIDO2), monitored with alerts.</details>

**Q59.** An app registration secret keeps expiring and causing outages.
<details><summary>Answer</summary>Use a **managed identity**, **workload identity federation**, or a **certificate** in Key Vault with auto-renewal.</details>

**Q60.** SaaS app where users from any organisation can sign in.
<details><summary>Answer</summary>Register a **multitenant** app in Entra ID; each customer tenant gets a **service principal** after consent.</details>

**Q61.** Which OAuth flow for a background daemon with no user?
<details><summary>Answer</summary>**Client credentials** flow (application permissions).</details>

**Q62.** AKS pods need Key Vault access without secrets.
<details><summary>Answer</summary>**Microsoft Entra Workload ID** (federated credential on a managed identity) + Key Vault CSI driver.</details>

**Q63.** An on-prem server must access Azure Storage without secrets.
<details><summary>Answer</summary>Onboard it to **Azure Arc**; use the Arc-enabled server's **managed identity**.</details>

**Q64.** An app is throttled by Key Vault under heavy load.
<details><summary>Answer</summary>Cache secrets in the app (or via App Configuration Key Vault references), use one vault per app per environment, and spread load.</details>

**Q65.** Storage account keys must rotate every 90 days.
<details><summary>Answer</summary>Best: stop using keys (Entra ID auth, disable shared key). Otherwise Key Vault near-expiry **Event Grid** event → Function that regenerates and stores the key.</details>

## More monitoring
**Q66.** Alert when a VM stops sending heartbeats.
<details><summary>Answer</summary>**Log search alert** on the `Heartbeat` table (or VM availability metric alert).</details>

**Q67.** Monitor AKS nodes, pods and container logs with Prometheus metrics.
<details><summary>Answer</summary>**Container insights** + **Azure Monitor managed service for Prometheus** + **Azure Managed Grafana**.</details>

**Q68.** Check every 5 minutes, from 5 regions, that a website responds.
<details><summary>Answer</summary>Application Insights **availability (standard) test**.</details>

**Q69.** Distributed tracing across 10 microservices.
<details><summary>Answer</summary>**Application Insights** with **OpenTelemetry** (correlation, application map).</details>

**Q70.** Cut Log Analytics cost for verbose debug logs that are rarely queried.
<details><summary>Answer</summary>**Basic/Auxiliary** table plan, filter with DCR transformations, shorter retention.</details>

**Q71.** Steady 500 GB/day ingestion. How do you lower the price?
<details><summary>Answer</summary>Log Analytics **commitment tier**.</details>

**Q72.** Remove sensitive fields from logs before they are stored.
<details><summary>Answer</summary>**Data collection rule transformations** (KQL at ingestion time).</details>

**Q73.** Every new resource must automatically get a diagnostic setting.
<details><summary>Answer</summary>Azure Policy **DeployIfNotExists** initiative (built-in "Enable allLogs category group…").</details>

**Q74.** Record network flows in a VNet for compliance and analysis.
<details><summary>Answer</summary>**VNet flow logs** (Network Watcher) → Storage + **Traffic Analytics**.</details>

**Q75.** Does an NSG block traffic from a VM to an IP? Where does the traffic route?
<details><summary>Answer</summary>Network Watcher **IP flow verify** and **next hop**.</details>

**Q76.** Metrics or logs for near real-time autoscale decisions?
<details><summary>Answer</summary>**Metrics** (lightweight, near real-time). Logs are for rich analysis.</details>
