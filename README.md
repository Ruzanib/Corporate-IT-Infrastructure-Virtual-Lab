# Corporate IT Infrastructure Virtual Lab (Active Directory, Identity Management & Network Troubleshooting)

##  Project Overview
This project demonstrates the hands-on deployment of a fully functional enterprise network infrastructure built within a virtualized sandbox ecosystem. The goal is to simulate real-world system administration, centralized Identity & Access Management (IAM), and advanced network troubleshooting encountered in corporate IT environments.

##  Infrastructure Architecture & Tech Stack
* **Hypervisor:** Oracle VM VirtualBox
* **Server Operating System:** Windows Server (Configured as a Primary Domain Controller)
* **Client Operating System:** Windows 10 Pro (Domain-joined Workstation)
* **Core Network Services:** Active Directory Domain Services (AD DS), Domain Name System (DNS), Windows Defender Firewall.
* **Domain Topology Name:** `RozIT.local`


##  Implemented Phases & Deployment Steps

**1. Server Provisioning & Domain Promotion**
* Installed and configured **Windows Server** within VirtualBox, allocating static IPv4 configurations.
* Promoted the server instance to a **Domain Controller (DC)** by installing the **Active Directory Domain Services (AD DS)** role.
* Built and structured the root domain forest under the FQDN: `RozIT.local`.

 **2. Enterprise Logical Structuring (Identity & Access Management)**
Designed a scalable corporate hierarchy using **Active Directory Users and Computers (ADUC)** by deploying logical **Organizational Units (OUs)** and provisioning domain users:
* **IT Department OU:** Created and provisioned dedicated IT personnel accounts with specific administrative access.
* **HR Department OU:** Established a secure container for Human Resources staff accounts to manage departmental access.
* *Demonstrated proper provisioning, standard naming conventions, and password baselines for corporate identity management.*

**3. Client Provisioning & Network Isolation**
* Installed a **Windows 10 Pro** virtual machine to act as the corporate workstation.
* Isolated both machines inside a secure **Internal Network** adapter settings within VirtualBox to block external interference and simulate a private LAN.


# The Troubleshooting Challenge (Case Study: Resolving DNS Resolution Faults)

During the integration phase, a significant network communication barrier occurred when attempting to join the Windows 10 client to the `RozIT.local` domain. 

# The Problem:
* Running a `ping` command using the Server's static IP address (`192.168.10.10`) was successful (layer 3 connectivity was up).
* However, attempting to join the domain via the GUI or executing `ping RozIT.local` resulted in an **"Active Directory Domain Controller could not be contacted"** error and a CLI **`Could not find host`** fault.

 **Root Cause Analysis & Resolution:**
1. **DHCP Overwrite:** Discovered that the virtual adapter was implicitly pulling alternative configurations that mismatched the preferred DNS routing.
2. **IPv6 Interference:** Windows 10 was prioritizing automated IPv6 link-local queries over the designated legacy IPv4 primary DNS resolver.
3. **The Fix:** 
   * Deployed CLI network scripting tools (`netsh`) to bind static DNS rules.
   * Manually disabled **IPv6 bindings** within the network adapter properties (`ncpa.cpl`).
   * Hardcoded the **Preferred DNS Server** to explicitly point directly to the DC IP (`192.168.10.10`).
   * Executed `ipconfig /flushdns` to clear outdated resolver caches.

> **Result:** Name resolution stabilized immediately, returning consistent `Reply` streams from `RozIT.local`.


##  Lab Screenshots & Proof of Concept

### Active Directory Logical Structure (OUs & Users)
