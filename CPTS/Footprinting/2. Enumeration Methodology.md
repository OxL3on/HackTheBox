## **Enumeration Methodology**

Enumeration is about finding the **correct path**, not forcing one.

Enumeration is divided into three levels:

- **Infrastructure-based enumeration**
- **Host-based enumeration**
- **OS-based enumeration**

![](https://cdn.services-k8s.prod.aws.htb.systems/content/modules/112/enum-method33.png)

### The Six Layers

| Layer | Description | Information Categories |
| --- | --- | --- |
| **1. Internet Presence** | Identify the company's internet presence and externally accessible infrastructure. | Domains, Subdomains, vHosts, ASN, Netblocks, IP Addresses, Cloud Instances, Security Measures |
| **2. Gateway** | Identify security measures protecting the external and internal infrastructure. | Firewalls, DMZ, IPS/IDS, EDR, Proxies, NAC, Network Segmentation, VPN, Cloudflare |
| **3. Accessible Services** | Identify externally and internally accessible services and interfaces. | Service Type, Functionality, Configuration, Port, Version, Interface |
| **4. Processes** | Identify internal processes, data sources, and destinations related to services. | PID, Processed Data, Tasks, Source, Destination |
| **5. Privileges** | Identify permissions and privileges available within accessible services. | Groups, Users, Permissions, Restrictions, Environment |
| **6. OS Setup** | Identify the operating system configuration and internal setup. | OS Type, Patch Level, Network Configuration, OS Environment, Configuration Files, Sensitive Private Files |
