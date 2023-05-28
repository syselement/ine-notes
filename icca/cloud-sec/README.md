# 📒3. Cloud Identity, Security, and Compliance

> #### 📕 Learning Objectives
>
> * Basics aspects of managing cloud **resources** and related tasks
> * Cloud **access control** fundamentals

## Cloud Security

**Securing** cloud resources involves implementing measures and best practices to protect data, applications and infrastructure deployed in a cloud environment from unauthorized access, data breaches and other security threats. Cloud security is a shared responsibility between the **cloud service provider** (CSP) and the **cloud user** (customer).

### Shared Responsibility Model

🔗 [Shared Responsibility Model - Crowdstrike](https://www.crowdstrike.com/cybersecurity-101/cloud-security/shared-responsibility-model/)

![Shared Responsibility Model - Crowdstrike](.gitbook/assets/image-20230528143915462.png)

|     Cloud Architecture      | e.g. Responsibility for IaaS | e.g. Responsibility for PaaS |
| :-------------------------: | ---------------------------- | ---------------------------- |
|          Workload           | User                         | User                         |
|          Services           | User                         | CSP                          |
|      Virtual Machines       | User                         | CSP                          |
|       *Control Plane*       | CSP                          | CSP                          |
|     **Virtualization**      | CSP                          | CSP                          |
| **Physical Infrastructure** | CSP                          | CSP                          |
|    **Physical Facility**    | CSP                          | CSP                          |

From a security standpoint, the responsibility depends on what level of service is used.

A the level of **data plane** and **control plane** (tools, consoles, CLI, SDK), securing cloud resources is important and IAM is a key aspect of it.

- Identity protection
- Strong authentication mechanisms
- Control access
- Data encryption
- Network security
- Patching and updates

**Security measures must be applied to both the data and the control plane.**

### Defense in depth

[Defense in depth](https://www.cloudflare.com/learning/security/glossary/what-is-defense-in-depth/) (layered security) is a principle and strategy in cloud security that involves implementing multiple layers of security controls and measures to protect cloud resources from various threats and attacks.

- Robust and resilient posture
- Mitigate the risk of a single security control

Public Network (Perimeter)

- Public **firewall**, DDos Prevention, IDS/IPS, etc

Local Network

- nACL, Device **Hardening**, Monitoring, etc

Operating System (Endpoint)

- Hardening, **Patching**, Endpoint Protection, **Monitoring**, etc

Service (Application)

- **Hardening**, Patching, Monitoring, Vuln Scanning, Testing, etc

Workload

- Authentication, Authorization, Auditing, Data access control, Monitoring, **Encryption** (in transit & at rest), MFA, etc

![Defence in Depth Infographic - colohouse.com](.gitbook/assets/Defense-in-Depth-Infographic-2022.jpg)

![Cloud Defence-in-Depth Concept - cyber.gc.ca](.gitbook/assets/itsp50104-Fig3.png)

🔗 `e.g.` [Google Cloud networking in depth](https://cloud.google.com/blog/products/networking/google-cloud-networking-in-depth-three-defense-in-depth-principles-for-securing-your-environment)

![Overview of network security controls - GCP](.gitbook/assets/network-security-controls-GCP.png)

## Cloud Attacks







## Identity Protection



### Response







## Resource Protection

### Data





### Network





### Compute





### Compliance









***

