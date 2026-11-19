# Module 4: Describe Azure identity, access, and security
Units: Introduction · Directory services · Authentication methods · External identities · Conditional Access · RBAC · Zero Trust · Defense-in-depth · Encryption and key management · Microsoft Defender for Cloud · Assessment · Summary

## Directory services
- **Microsoft Entra ID** (formerly Azure AD): cloud identity and access management. Provides authentication, SSO, application management, device management, and hybrid identity.
- **Microsoft Entra Connect**: syncs on-premises AD DS users to Entra ID.
- **Microsoft Entra Domain Services**: managed domain services (domain join, group policy, LDAP, Kerberos/NTLM) **without deploying domain controllers**. Good for lifting legacy apps that need Kerberos/LDAP. One-way sync from Entra ID.

## Authentication methods
| Method | Notes |
|---|---|
| **Single sign-on (SSO)** | One identity for many apps |
| **MFA** | Something you know + something you have + something you are |
| **Passwordless** | Windows Hello for Business, Microsoft Authenticator, FIDO2 security keys, passkeys |

## External identities
- **B2B collaboration**: invite partners as guests; they sign in with their own credentials.
- **B2B direct connect**: mutual two-way trust with another Entra organisation (for example Teams shared channels).
- **Azure AD B2C / Microsoft Entra External ID**: customer-facing (CIAM) apps, social and local accounts.

## Conditional Access
```mermaid
flowchart LR
  S[Signals: user, group, location, device, app, risk] --> D{Policy decision}
  D -->|Grant| G[Allow]
  D -->|Grant with controls| M[Require MFA / compliant device]
  D -->|Block| B[Block access]
```
- If-then policies. Requires **Microsoft Entra ID P1** (risk-based policies need **P2**).

## Azure RBAC
- **Security principal** (user, group, service principal, managed identity) + **role definition** + **scope** = role assignment.
- Scopes: management group → subscription → resource group → resource. Assignments are **inherited** downward.
- Built-in roles: **Owner** (all, including access), **Contributor** (all except granting access), **Reader**, **User Access Administrator** (manage access only).
- RBAC is an **allow** model; Azure **deny assignments** exist (for example created by Blueprints/managed apps).

## Zero Trust
Three principles: **Verify explicitly**, **use least-privilege access**, **assume breach**. Never trust based on the network location.

## Defense-in-depth (7 layers, outside → inside)
Physical security → Identity & access → Perimeter (DDoS, firewall) → Network (segmentation, NSG) → Compute (patching, endpoint protection) → Application (secure code, secrets) → **Data**.

## Encryption and key management
- Encryption at rest is on by default (Storage Service Encryption, Transparent Data Encryption for SQL).
- Azure Disk Encryption (BitLocker/DM-Crypt) for VM disks.
- **Azure Key Vault**: store secrets, keys and certificates; HSM-backed options (Premium tier, Managed HSM).

## Microsoft Defender for Cloud
- **CSPM** (posture management): Secure Score and recommendations.
- **CWPP** (workload protection): Defender plans for servers, containers, SQL, storage, Key Vault and more.
- Covers Azure, on premises (via **Azure Arc**) and other clouds (AWS, GCP).
- Regulatory compliance dashboard (Microsoft cloud security benchmark, ISO, PCI…).

## Self-check
1. Legacy app needs LDAP/Kerberos but you do not want to manage DCs → **Microsoft Entra Domain Services**
2. Require MFA only when users sign in from outside the corporate network → **Conditional Access**
3. A user must manage resources but not grant access to others → **Contributor**
4. Which Zero Trust principle says "minimise blast radius"? → **Assume breach**
