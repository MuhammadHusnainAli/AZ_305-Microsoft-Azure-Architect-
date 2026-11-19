# Module 1: Microsoft Azure Well-Architected Framework — introduction
Units: Introduction · Framework overview (pillars) · Workload and design principles · Trade-offs · Well-Architected Review and tools · Knowledge check · Summary

```mermaid
flowchart TB
  W[Workload] --- R[Reliability] & S[Security] & C[Cost Optimization] & O[Operational Excellence] & P[Performance Efficiency]
  R & S & C & O & P --> T[Balance trade-offs against business requirements]
```

![An illustration that shows the pillars of the Azure Well-Architected Framework.](../assets/06-Well-Architected-Framework/01-WAF-Introduction/2-pillars.png)
*An illustration that shows the pillars of the Azure Well-Architected Framework.*

![An illustration that shows two virtual machines in a virtual network. One of the machines is shown as failed, while the other is working to service customer requests.](../assets/06-Well-Architected-Framework/01-WAF-Introduction/2-system-failure.png)
*An illustration that shows two virtual machines in a virtual network. One of the machines is shown as failed, while the other is working to service customer requests.*

![An illustration that shows the types of security threats and attacks that might affect your data in the cloud.](../assets/06-Well-Architected-Framework/01-WAF-Introduction/2-security.png)
*An illustration that shows the types of security threats and attacks that might affect your data in the cloud.*

![An illustration that shows increasing quality, speed, and efficiency while maintaining decreasing costs.](../assets/06-Well-Architected-Framework/01-WAF-Introduction/2-efficiency.png)
*An illustration that shows increasing quality, speed, and efficiency while maintaining decreasing costs.*

![An illustration that shows how resources in the cloud scale dynamically based on demand, resulting in highly efficient usage. When resources are implemented at a fixed level, it results in inefficient usage during low demand and shortage during high demand.](../assets/06-Well-Architected-Framework/01-WAF-Introduction/2-performance-demand.png)
*An illustration that shows how resources in the cloud scale dynamically based on demand, resulting in highly efficient usage. When resources are implemented at a fixed level, it results in inefficient usage during low demand and shortage during high demand.*

## Key ideas
- A **workload** = the set of application resources, data and supporting infrastructure that deliver a business outcome.
- Each pillar provides: **design principles**, a **design review checklist**, **trade-offs**, **recommendations** and **patterns**.
- Built on the **shared responsibility model**: the more PaaS/SaaS you use, the more Microsoft handles.

## Pillar summary
| Pillar | One-line goal | Typical questions |
|---|---|---|
| Reliability | Resilient and recovers to a fully working state | What are your RTO/RPO? What fails if a zone goes down? |
| Security | Protect confidentiality, integrity, availability | Who can access what? Is data encrypted? |
| Cost Optimization | Maximise value within budget | Are resources right-sized? Who owns the spend? |
| Operational Excellence | Deploy and operate with confidence | Is everything automated, monitored, and repeatable? |
| Performance Efficiency | Scale efficiently with demand | What happens under 10× load? |

## Tools
| Tool | Use |
|---|---|
| **Azure Well-Architected Review** | Self-assessment that produces a score and recommendations |
| **Azure Advisor** | Recommendations for deployed resources by pillar; **Advisor Score** |
| **WAF service guides** | Per-service guidance (for example AKS, App Service, SQL) |
| **Azure Architecture Center** | Reference architectures, design patterns |
| **Azure Proactive Resiliency Library / Workbooks** | Resiliency checks |

## Trade-off examples
| Improving… | May hurt… |
|---|---|
| Reliability (multi-region) | Cost, operational complexity |
| Security (inspection, encryption) | Performance, cost |
| Cost (fewer instances) | Reliability, performance |
| Performance (overprovisioning, caching) | Cost |

## Self-check
1. Which tool scores existing deployed resources by WAF pillar? → **Azure Advisor**
2. What is a "workload"? → **Resources, data and infrastructure that together deliver a business outcome**
