# Module 6: Introduction to the Microsoft Azure Well-Architected Framework (WAF)
Units: Introduction · Azure Well-Architected Framework pillars · Summary

The WAF is a set of guiding tenets for improving the quality of a **workload**. Each pillar has design principles, checklists, trade-offs, and recommendations. The full deep dive is in [folder 06](../06-Well-Architected-Framework).

```mermaid
mindmap
  root((Well-Architected<br/>Framework))
    Reliability
      Resilient, available, recovers
    Security
      Confidentiality, integrity, availability
    Cost Optimization
      Maximise value per cost
    Operational Excellence
      DevOps, automation, observability
    Performance Efficiency
      Scale to meet demand
```

| Pillar | Goal | Example design choices |
|---|---|---|
| **Reliability** | The workload meets its uptime and recovery targets | Availability zones, multi-region, health probes, retries, backup |
| **Security** | Protect against threats (CIA triad) | Zero Trust, Entra ID, managed identities, encryption, Key Vault, WAF/Firewall |
| **Cost Optimization** | Deliver business value at the lowest cost | Right-sizing, reservations, autoscale, Spot VMs, budgets |
| **Operational Excellence** | Deploy and run reliably and safely | IaC, CI/CD, monitoring, safe deployment practices |
| **Performance Efficiency** | Adapt efficiently to changes in demand | Scale out, caching, CDN, partitioning, load testing |

![An illustration that shows the pillars of the Azure Well-Architected Framework.](../assets/01-AZ-305-Prerequisites/06-Well-Architected-Framework-Intro/2-pillars.png)
*An illustration that shows the pillars of the Azure Well-Architected Framework.*

![An illustration that shows two virtual machines in a virtual network. One of the machines is shown as failed, while the other is working to service customer requests.](../assets/01-AZ-305-Prerequisites/06-Well-Architected-Framework-Intro/2-system-failure.png)
*An illustration that shows two virtual machines in a virtual network. One of the machines is shown as failed, while the other is working to service customer requests.*

![An illustration that shows the types of security threats and attacks that might affect your data in the cloud.](../assets/01-AZ-305-Prerequisites/06-Well-Architected-Framework-Intro/2-security.png)
*An illustration that shows the types of security threats and attacks that might affect your data in the cloud.*

![An illustration that shows increasing quality, speed, and efficiency while maintaining decreasing costs.](../assets/01-AZ-305-Prerequisites/06-Well-Architected-Framework-Intro/2-efficiency.png)
*An illustration that shows increasing quality, speed, and efficiency while maintaining decreasing costs.*

![An illustration that shows how resources in the cloud scale dynamically based on demand, resulting in highly efficient usage. When resources are implemented at a fixed level, it results in inefficient usage during low demand and shortage during high demand.](../assets/01-AZ-305-Prerequisites/06-Well-Architected-Framework-Intro/2-performance-demand.png)
*An illustration that shows how resources in the cloud scale dynamically based on demand, resulting in highly efficient usage. When resources are implemented at a fixed level, it results in inefficient usage during low demand and shortage during high demand.*

## Tools
- **Azure Well-Architected Review**: an assessment questionnaire that scores your workload.
- **Azure Advisor**: personalised recommendations grouped by the same five pillars.
- **Azure Architecture Center**: reference architectures and patterns.

## Trade-offs
Pillars can conflict. Example: multi-region improves reliability but raises cost; more security inspection can add latency. Architects balance them against business requirements.

## Self-check
1. Which tool gives recommendations by WAF pillar for existing resources? → **Azure Advisor**
2. Autoscaling to meet peak demand belongs mainly to which pillar? → **Performance Efficiency**
3. Infrastructure as code and CI/CD belong mainly to? → **Operational Excellence**
