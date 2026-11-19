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
