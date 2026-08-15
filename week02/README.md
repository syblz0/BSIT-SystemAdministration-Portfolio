# Enterprise Infrastructure Planning for a Startup Company

**Course:** ITEP 414 – System Administration and Maintenance  
**Week:** 02  
**Prepared by:** Syron B. Blaza  
**Date:** August 14, 2026  

---

##  Project Overview

This project simulates the role of a Junior System Administrator tasked with designing a complete IT infrastructure plan for a newly established startup, **NovaStack Solutions**, from the ground up — covering hardware, software, networking, staffing roles, and security recommendations before any equipment is purchased.

The full report is available here: [`EnterpriseInfrastructurePlan.pdf`](./EnterpriseInfrastructurePlan.pdf)

---

##  Learning Objectives

By completing this project, I practiced how to:
- Explain the roles and responsibilities of a System Administrator
- Identify hardware, software, and networking requirements for a small business
- Analyze organizational IT requirements
- Prepare professional IT inventories
- Design an enterprise network topology
- Create technical documentation using Markdown
- Present infrastructure planning in a professional format

---

## Company Scenario

**NovaStack Solutions** is a fictional software development startup with **20 employees** across four departments:

| Department | Employees |
|---|---|
| Information Technology | 5 |
| Human Resources | 4 |
| Finance | 5 |
| Sales | 6 |
| **Total** | **20** |

The company currently has **no computers, server, network, internet infrastructure, or security policies** — this project designs everything from scratch.

---

##  Hardware Inventory Summary

| Hardware | Quantity | Purpose |
|---|---|---|
| Desktop Computer | 10 | IT & Finance workstations |
| Laptop | 10 | HR & Sales mobility |
| Server | 1 | Hosts internal apps & file sharing |
| Router | 1 | Manages internet traffic |
| Switch | 2 | Distributes network to departments |
| Printer | 4 | One per department |
| UPS | 3 | Power protection |
| Wireless Access Point | 2 | Office Wi-Fi coverage |
| NAS Storage | 1 | Centralized backups |
| External Backup Drive | 2 | Offline backup |
| Monitor | 10 | Paired with desktops |

*Full breakdown with Asset IDs and justifications available in the [full report](./EnterpriseInfrastructurePlan.pdf).*

---

## Software Inventory Summary

| Software | Purpose |
|---|---|
| Windows 11 Pro | Main OS for workstations |
| Ubuntu Server | Server OS for internal apps |
| Microsoft 365 | Productivity & collaboration |
| VS Code, Git, GitHub Desktop | Development tools |
| VirtualBox | Testing & virtualization |
| Google Chrome | Web browser |
| Microsoft Defender | Antivirus/endpoint protection |
| AnyDesk | Remote IT support |
| 7-Zip | File compression |

*Full version, license, and purpose details available in the [full report](./EnterpriseInfrastructurePlan.pdf).*

---

## Network Diagram

![NovaStack Solutions Network Topology](./diagrams/network-diagram.png)

The topology follows: **Internet → ISP Modem → Router → Firewall → Core Switch → (Server, Printer, Wireless AP, Distribution Switch) → IT / HR / Finance / Sales Departments**

[View full diagram as PDF](./diagrams/network-diagram.pdf)

---

##  Technologies Used

- **draw.io** – network diagram design
- **Markdown** – technical documentation
- **Microsoft Word** – final report formatting
- **GitHub** – version control & portfolio hosting

---

##  Challenges Encountered

The most challenging part of this project was designing the network diagram logically — initially connecting shared devices (server, printer, wireless AP) directly to individual departments, then realizing a distribution switch was needed to properly fan out the connection to all four departments instead.

---

##  Reflection

This project shifted how I think about system administration — from reactive troubleshooting to proactive planning. Mapping out departments, hardware, software, and network flow together showed me how interconnected every infrastructure decision really is, and why planning before deployment saves both time and cost. Full reflection available in the [full report](./EnterpriseInfrastructurePlan.pdf).

---

##  References

See [`references/references.md`](./references/references.md) for full source list.
