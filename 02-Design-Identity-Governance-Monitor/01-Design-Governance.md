# Module 1: Design governance
Units: Introduction · Design for governance · Design for management groups · Design for subscriptions · Design for resource groups · Design for resource tags · Design for Azure Policy · Design for RBAC · Design for Azure landing zones · Knowledge check · Summary

Exam skills: recommend a structure for management groups, subscriptions, resource groups and tagging · recommend a compliance solution · recommend an identity governance solution.

```mermaid
flowchart TB
  R[Tenant root group] --> P[Platform MG]
  R --> LZ[Landing zones MG]
  R --> SB[Sandbox MG]
  R --> DC[Decommissioned MG]
  P --> ID[Identity sub] & CON[Connectivity sub] & MGT[Management sub]
  LZ --> CORP[Corp MG] & ONL[Online MG]
  CORP --> APP1[App subscription] --> RG[Resource groups] --> RES[Resources]
```
Policy and RBAC flow **down** this tree. Assign as **high** as possible, as **narrow** as needed.

![Diagram of the Azure hierarchy that shows the tenant root group, management groups, subscriptions, resource groups, and resources.](../assets/02-Design-Identity-Governance-Monitor/01-Design-Governance/2-governance-strategies.png)
*Diagram of the Azure hierarchy that shows the tenant root group, management groups, subscriptions, resource groups, and resources.*

## Management groups
- Organise subscriptions by **governance requirements**, not by org chart. Keep the hierarchy shallow (3–4 levels recommended; the limit is 6).
- Apply policy at management group level (for example "allowed regions" for all of production).
- Root management group: don't assign directly to it unless needed; changes to it require elevated permissions.

![Diagram of management groups for the sales, corporate, and IT departments.](../assets/02-Design-Identity-Governance-Monitor/01-Design-Governance/3-management-groups.png)
*Diagram of management groups for the sales, corporate, and IT departments.*

## Subscriptions
- Use them as **units of management, billing and scale**. Reasons to split: different environments, cost centres, compliance boundaries, scale limits, network isolation.
- Create a **dedicated subscription for shared platform services** (identity, connectivity hub, management/logging).
- Subscription quotas are a hard limit: design for scale-out across subscriptions.

![Diagram of Microsoft Entra ID showing the subscription types - dev, test, and production.](../assets/02-Design-Identity-Governance-Monitor/01-Design-Governance/4-azure-active-directory-subscriptions.png)
*Diagram of Microsoft Entra ID showing the subscription types - dev, test, and production.*

![Diagram that shows subscription options for Tailwind Traders departments, such as sales, corporate, and IT.](../assets/02-Design-Identity-Governance-Monitor/01-Design-Governance/4-subscriptions-example.png)
*Diagram that shows subscription options for Tailwind Traders departments, such as sales, corporate, and IT.*

## Resource groups
- Put resources with the **same lifecycle** together (deploy, update and delete as one).
- An RG has a location (where its metadata is stored), but resources can be in other regions.
- Use RGs as a scope for RBAC and as a unit for deletion.

![Diagram that shows the resource groups for the production applications.](../assets/02-Design-Identity-Governance-Monitor/01-Design-Governance/5-resource-groups.png)
*Diagram that shows the resource groups for the production applications.*

![Diagram that shows separate resource groups for the database and web applications.](../assets/02-Design-Identity-Governance-Monitor/01-Design-Governance/5-group-type.png)
*Diagram that shows separate resource groups for the database and web applications.*

![Diagram that shows separate resource groups for App1 and App2.](../assets/02-Design-Identity-Governance-Monitor/01-Design-Governance/5-group-app.png)
*Diagram that shows separate resource groups for App1 and App2.*

## Resource tags
- Name/value pairs for **cost reporting, ownership, environment, criticality**. Example: `CostCenter`, `Env`, `Owner`, `DataClassification`.
- Tags are **not inherited** by default → use **Azure Policy** (`Inherit a tag from the resource group`, `Require a tag`) to enforce them.
- Limit: 50 tags per resource.

![Diagram that shows an example hierarchy of resource tags.](../assets/02-Design-Identity-Governance-Monitor/01-Design-Governance/6-resource-tags.png)
*Diagram that shows an example hierarchy of resource tags.*

## Azure Policy
- Evaluates resources for compliance with rules. Effects: **Deny**, **Audit**, **Append**, **Modify**, **DeployIfNotExists**, **AuditIfNotExists**, **Disabled**, (**DenyAction**, **Manual**).
- **Initiative** (policy set): a group of policies with one goal (for example ISO 27001, Microsoft cloud security benchmark).
- **Remediation tasks** fix existing non-compliant resources (for `Modify`/`DeployIfNotExists`).
- Policy applies to new and existing resources; existing ones are marked non-compliant, not deleted.

| Azure Policy | Azure RBAC |
|---|---|
| Controls **what** can be deployed (properties) | Controls **who** can do actions |
| Default allow, rules deny | Default deny, roles allow |

![Diagram that shows different Azure policies applied at the Production and Application resource group levels.](../assets/02-Design-Identity-Governance-Monitor/01-Design-Governance/7-azure-policy-choices.png)
*Diagram that shows different Azure policies applied at the Production and Application resource group levels.*

## Azure RBAC design
- Assign roles to **groups**, not individuals. Use **least privilege**.
- Use **custom roles** when built-in ones are too broad.
- Use **Privileged Identity Management (PIM)** for just-in-time elevation (Entra ID P2).

![Diagram that shows how a role definition is assigned to a resource and then scoped.](../assets/02-Design-Identity-Governance-Monitor/01-Design-Governance/8-role-definition-assignment-scope.png)
*Diagram that shows how a role definition is assigned to a resource and then scoped.*

![Diagram that shows how to plan roles of different scope levels within the organization.](../assets/02-Design-Identity-Governance-Monitor/01-Design-Governance/8-scope-role-matrix.png)
*Diagram that shows how to plan roles of different scope levels within the organization.*

## Azure landing zones
- Use the CAF landing-zone architecture (shown in the diagram) as the reference.
- Start options: **Azure landing zone accelerator** (portal), Bicep/Terraform modules.
- Separate **platform** (identity, management, connectivity) from **application** landing zones.

![Diagram of a management group and subscription organization that uses landing zones.](../assets/02-Design-Identity-Governance-Monitor/01-Design-Governance/9-landing-zones.png)
*Diagram of a management group and subscription organization that uses landing zones.*

## Compliance and identity governance
- **Microsoft Defender for Cloud → Regulatory compliance** and **Azure Policy initiatives** → continuous compliance view.
- **Microsoft Purview**: data governance, data map and classification.
- **Resource locks**: `CanNotDelete` / `ReadOnly`, applied to subscriptions, RGs or resources; they override RBAC permissions.
- **Identity governance** (Entra ID P2 / Entra ID Governance): **access reviews**, **entitlement management** (access packages), **PIM**, lifecycle workflows.

## Exam tips
- "Prevent deployment of resources in non-approved regions" → **Azure Policy (Deny)** at management group.
- "Ensure every resource has a CostCenter tag copied from the RG" → **Policy with the Modify effect**.
- "Prevent accidental deletion" → **Resource lock (CanNotDelete)**.
- "Periodically confirm guests still need access" → **Access reviews**.
- "Give admins temporary elevated rights with approval" → **PIM**.
