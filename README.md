# 🏢 Enterprise Environment Project: 🪟Windows  
**Active Directory | Group Policy | RBAC | Endpoint Management**

---

# 🧪 Lab 1 – Active Directory & Identity Management  
*(Windows Server AD DS Deployment & RBAC Design)*

## 🔎 Overview

In this lab, I deployed and configured a Windows Server Domain Controller (`DC01`) running **Active Directory Domain Services** for the domain `lab.local`.  

I designed a scalable Organizational Unit (OU) hierarchy and implemented **Role-Based Access Control (RBAC)** using Global Security Groups.

This lab demonstrates:

- Identity and Access Management (IAM)
- Logical AD structure design
- Enterprise RBAC principles
- Least-privilege security implementation

---

## 🏗 Environment Components

- **Windows Server (DC01)**
- Active Directory Domain Services
- DNS (AD-integrated)
- DHCP
- Windows 11 Pro (Domain Joined)
- Ubuntu Server (Network Integrated)

---

## 🧠 Organizational Unit (OU) Structure

To simulate an enterprise environment, I created the following OU layout:

Corp-Users

├── IT

├── HR

└── Finance

Corp-Computers

Corp-Groups

Service-Accounts


### ✅ Actions Performed

- Moved user **Kobe Bryant** into `Corp-Users > IT`
- Moved Windows 11 workstation into `Corp-Computers`
- Organized objects for clean Group Policy targeting
- Structured domain for scalability and administrative clarity

### 🎯 Why This Matters

Separating users, computers, and groups:

- Enables precise GPO targeting  
- Simplifies administration  
- Reflects real-world enterprise AD design  

---

## 🔐 Security Groups & RBAC Implementation

Created **Global Security Groups** inside `Corp-Groups`:

- `IT-Admins`
- `IT-Support`
- `Domain-Workstations`
- `VPN-Users`

### ⚙ Configuration Details

- **Group Type:** Security  
- **Group Scope:** Global  

### 👥 Membership Assignments

- Kobe Bryant → `IT-Support`
- Domain Admin → `IT-Admins`

### 🎯 Why This Matters

Permissions are assigned to **security groups**, not individual users — following enterprise RBAC and least-privilege principles.

---

## 📸 Evidence (Screenshots)

- Full OU structure in Active Directory Users and Computers <img width="1046" height="775" alt="01_ADUC_org" src="https://github.com/user-attachments/assets/c723d86f-02ac-4267-9bd9-7e438034e797" />
- User located in IT OU  <img width="1049" height="810" alt="02_Kobe_in_IT_OU" src="https://github.com/user-attachments/assets/9b65309c-192a-4e68-93e5-4d3502f251c6" />
- Windows 11 computer object in Corp-Computers OU <img width="1045" height="784" alt="03_computerOU" src="https://github.com/user-attachments/assets/b6f61f84-e188-4d5f-b789-3455cd22e47d" />
- Security group configuration (Global + Security) <img width="1054" height="781" alt="04_security_groups" src="https://github.com/user-attachments/assets/02661060-f132-4ce8-9d7d-270591a39c6d" />   
- Group membership assignments <img width="1055" height="800" alt="05_ITadmin_members" src="https://github.com/user-attachments/assets/bdbcfa16-8f50-47db-b39f-713f367a55f9" />

---

---

# 🧪 Lab 2 – Group Policy & Endpoint Management  
*(Security Baseline & Centralized Workstation Control)*

## 🔎 Overview

In this lab, I implemented multiple **Group Policy Objects (GPOs)** to enforce domain-wide security policies and centrally manage workstation configurations.

This demonstrates:

- Enterprise endpoint management
- Authentication security enforcement
- Centralized privilege control
- GPO design and deployment

---

## 🔐 Domain-Wide Baseline Security GPO

**GPO Name:** `Baseline-Security`  
**Linked At:** `lab.local` (Domain Level)

### ⚙ Configurations Implemented

- Password complexity enabled  
- Minimum password length: **12 characters**  
- Account lockout threshold: **5 invalid attempts**  
- Lockout duration: **15 minutes**  

### 🎯 Impact

- Enforces secure authentication standards  
- Protects against brute-force login attempts  
- Applies consistently across the entire domain  

---

## 📸 Evidence

- GPO linked at domain level <img width="1061" height="801" alt="06_baseline_security" src="https://github.com/user-attachments/assets/19a96927-16e6-4384-8cab-ac70c176179e" />
- Account lockout configuration <img width="1073" height="832" alt="07_accountpolicy" src="https://github.com/user-attachments/assets/710398e2-562c-4635-b79c-31a1eff26257" />
- Password policy settings <img width="1048" height="793" alt="08_password_policy" src="https://github.com/user-attachments/assets/fb417081-89b8-45c1-bf09-c73c67aa9294" />

---

## 🖥 Workstation User Restrictions GPO

**GPO Name:** `Workstation-User-Restrictions`  
**Linked To:** `Corp-Users` OU  

### ⚙ Configurations Implemented

- Disabled Control Panel access  
- Disabled Command Prompt  
- Restricted specific user configuration settings  

### 🧪 Validation

Logged into Windows 11 as **Kobe Bryant** and confirmed restrictions were enforced.

### 🎯 Impact

Demonstrates:

- Centralized workstation control  
- Policy enforcement at the OU level  
- Real-world endpoint restriction management
  
---

## 📸 Evidence

- User Configuration GPO settings <img width="1066" height="774" alt="09_user_restriction" src="https://github.com/user-attachments/assets/5f080ccc-4056-47dc-b5b9-2972595b1f43" />
  
- Control Panel disabled on Windows 11 <img width="1035" height="809" alt="10_win11_no_control_panel" src="https://github.com/user-attachments/assets/50ac6d09-2e30-4792-aef0-2030606d1742" />
  
---

## 👮 Local Administrator Control via Group Policy

To enforce **least privilege**, I implemented centralized local administrator management.

### 🛠 Steps Implemented

1. Created Security Group: `Workstation-LocalAdmins`
2. Nested `IT-Admins` inside `Workstation-LocalAdmins`
3. Configured GPO: 
Computer Configuration
→ Preferences
→ Control Panel Settings
→ Local Users and Groups
-   Action: **Replace**
-   Target: **Administrators Group**
-   Added: `Workstation-LocalAdmins`

4. Linked GPO to: `Corp-Computers`

---

### 🧪 Validation

- Standard IT-Support user (Kobe) is **NOT** a local administrator  
- IT-Admins group members **ARE** local administrators  

Verified using: CLI

- `whoami /groups`
- `net localgroup administrators`

---

### 🎯 Impact

- Prevents unauthorized elevation of privileges  
- Enforces enterprise least-privilege standards  
- Demonstrates real-world workstation privilege management  

---

## 📸 Evidence

- Local Users and Groups GPO configuration <img width="1038" height="831" alt="11_localadmin_workstationGPO" src="https://github.com/user-attachments/assets/3fa73d0f-e970-4fde-af58-0868e0fe76f5" />

- Local group verification on Win11 <img width="1033" height="819" alt="12_win11_groups_whoami" src="https://github.com/user-attachments/assets/4f770f7b-12f9-42d1-9c70-ba4ed6820fa4" />
 

---

# 🚀 Skills Demonstrated Across Labs 1 & 2

- Active Directory administration  
- OU and object organization  
- Role-Based Access Control (RBAC)  
- Domain-wide password policy enforcement  
- Workstation restriction via GPO  
- Centralized local administrator management  
- Enterprise security baseline implementation  

---
---

# 🧪 Lab 3 – File Services & NTFS Permission Management  
*(Department-Based Access Control & Drive Mapping)*

## 🔎 Overview

In this lab, I configured centralized file shares on the domain controller and implemented department-based access control using **NTFS permissions and Active Directory security groups**.

This demonstrates:

- File server administration
- NTFS vs Share permission design
- Group-based access control
- Centralized drive mapping via GPO

---

## 🗂 Department File Share Structure

Created the following folder structure on `DC01`:

D:\IT-Share

D:\HR

D:\Finance

---

## 🔐 Permission Configuration

### ⚙ Share-Level Permissions

- Removed default broad access (e.g., Everyone)
- Granted access only to appropriate security groups

### ⚙ NTFS Permissions

Configured permissions using AD Security Groups:

- `IT-Support` → Modify access to `IT-Share`
- Department groups assigned to their respective folders
- Inheritance reviewed and adjusted where necessary

---

### 🎯 Why This Matters

- Enforces department-based data separation
- Prevents unauthorized file access
- Follows enterprise best practice: **Access via Groups, not Users**

---

## 🗺 Drive Mapping via Group Policy

Created GPO: `Map-IT-Drive`

### Configuration Path: 
User Configuration
→ Preferences
→ Windows Settings
→ Drive Maps


Mapped:

- Drive Letter: `I:`
- Location: `\\DC01\IT-Share`

Security Filtering:
- Applied to `IT-Support` group

---

### 🧪 Validation

- Logged in as Kobe Bryant
- Verified automatic drive mapping
- Confirmed correct access permissions
- Tested access denial for unauthorized folders

---

## 📸 Evidence

- NTFS Security tab configuration <img width="1031" height="809" alt="13_SHARES_nfts_security" src="https://github.com/user-attachments/assets/8612a6fc-be79-4287-b200-9fcb50c0bdbe" />
- Share-level permissions <img width="1034" height="811" alt="14_share_permissions" src="https://github.com/user-attachments/assets/60d18109-3042-41fd-a6ba-7e9573bf95f3" />
- Drive mapping GPO configuration <img width="1041" height="821" alt="15_Map_IT_driveGPO" src="https://github.com/user-attachments/assets/cef552b2-af5c-449e-9d13-eec12b5c4d05" />
-  Mapped network drive visible in Windows 11  <img width="1038" height="800" alt="16_win11_mapped_drive" src="https://github.com/user-attachments/assets/d6f79809-682a-4e3b-8142-dc4cf95bf813" />

---

# 🚀 Skills Demonstrated Across Lab 3

- SMB file sharing configuration  
- NTFS permission management  
- AD group-based access control  
- GPO-based drive mapping  
- File access troubleshooting  

---

---

# 🧪 Lab 4 – DNS & DHCP Infrastructure Services  
*(Core Domain Networking & Troubleshooting)*

## 🔎 Overview

In this lab, I configured and validated **DHCP and DNS services integrated with Active Directory**, ensuring reliable domain communication and automatic IP address management for Windows and Linux systems.

This lab demonstrates:

- Core networking fundamentals
- AD-integrated DNS configuration
- DHCP scope creation and authorization
- Structured troubleshooting methodology

---

## 🌐 DHCP Configuration

Installed and configured the DHCP role on `DC01`.

### ⚙ Scope Configuration

- Created IPv4 scope
- Configured IP address range
- Defined subnet mask
- Configured DNS server option (pointing to DC01)
- Authorized DHCP server in Active Directory

---

### 🧪 Validation

- Windows 11 obtained IP automatically
- Ubuntu Server obtained IP via DHCP
- Verified via:

ipconfig /all

---

## 📸 Evidence

- DHCP scope configuration <img width="1035" height="821" alt="17_dhcp_scope_config" src="https://github.com/user-attachments/assets/71fd497d-cbe5-4c23-98a5-6bf6f9437266" />

---

## 🧭 DNS Configuration

Verified AD-integrated DNS functionality:

- Forward Lookup Zone: `lab.local`
- Dynamic registration enabled
- Confirmed domain controllers registered properly

---

### 🧪 DNS Testing

Validated name resolution using: CLI

- `nslookup DC01`
- `ping DC01`
Confirmed proper DNS resolution from:

- Windows 11 client
- Ubuntu server

---

## 🔧 DNS Failure Simulation & Troubleshooting

To simulate a real-world help desk scenario:

1. Manually misconfigured DNS server on Windows 11
2. Observed:
   - Domain login failures
   - Name resolution failure
3. Diagnosed using:
   - `ipconfig`
   - `nslookup`
4. Restored correct DNS server setting
5. Verified connectivity restored

---

### 🎯 Why This Matters

Active Directory is **DNS-dependent**.  
Understanding DNS failures is critical for:

- Login troubleshooting
- GPO application issues
- Domain communication problems

---

## 📸 Evidence

- DNS forward lookup zone <img width="1042" height="819" alt="18_dns_forward_zone" src="https://github.com/user-attachments/assets/5b5e61fb-c3e7-4374-9bcc-0bdee3c8c6d2" />
- Client DNS configuration <img width="1035" height="817" alt="19_ipconfig_all" src="https://github.com/user-attachments/assets/4c335079-23f0-4f29-832b-e8702dbc7696" />
- DNS misconfiguration causing failure <img width="1036" height="813" alt="20_win11_dns_MISconfig" src="https://github.com/user-attachments/assets/4ba6767e-3a4e-4b5a-b22e-ce399a66a984" />
- Connectivity restored <img width="1036" height="804" alt="21_connection_restored" src="https://github.com/user-attachments/assets/e0b1b9e5-8c60-4f7f-b98b-cfd397fd8da7" />

---

# 🚀 Skills Demonstrated Across Lab 4

- DHCP role installation & configuration  
- IP scope design  
- DNS zone validation  
- Name resolution testing  
- Network troubleshooting  
- Root cause analysis  

---
---

# 🧪 Lab 5 – Linux Server Administration  
*(Cross-Platform Systems Management & CLI Operations)*

## 🔎 Overview

In this lab, I deployed an Ubuntu Server within the Active Directory lab network to demonstrate cross-platform system administration and Linux command-line proficiency.

This lab demonstrates:

- Linux system deployment
- User and permission management
- SSH configuration
- Cross-platform connectivity
- Basic privilege delegation (sudo)

---

## 🖥 Linux Environment Setup

- Installed Ubuntu Server
- Configured network via DHCP (assigned by DC01)
- Verified domain network connectivity
- Confirmed DNS resolution to domain controller

Validated connectivity using: CLI

- `ping DC01`
- `ip addr`
  
---

## 👤 User & Privilege Management

Created a new administrative user:

- `sudo adduser itadmin`
- `sudo usermod -aG sudo itadmin`


### Actions Performed

- Created local user `itadmin`
- Added user to `sudo` group
- Verified group membership
- Tested privilege escalation using `sudo`

---

### 🎯 Why This Matters

Demonstrates:

- Linux user account management
- Role-based privilege assignment
- Secure administrative delegation
- Command-line proficiency

---

## 🔐 File Permission Management

Performed ownership and permission modifications:

- `chmod 750 filename`
- `chown itadmin:itadmin filename`
- `ls -l`

### Validated:

- Correct file ownership
- Proper permission enforcement
- Restricted access for unauthorized users

---

### 🎯 Why This Matters

Understanding Linux permissions is critical for:

- Securing server environments
- Managing shared resources
- Preventing unauthorized access

---

## 🌐 SSH Configuration & Remote Access

Enabled and validated SSH access from Windows 11 workstation.

Verified:

- Successful remote login to Ubuntu server
- Secure CLI access
- Cross-platform network communication

---

## 📸 Evidence

- SSH session from Windows 11 to Ubuntu <img width="1021" height="807" alt="22_ssh_win11_toUBU" src="https://github.com/user-attachments/assets/a0ab1f2d-96ce-49b4-ad35-e4eb1033d141" />
- Verification of sudo group membership <img width="1025" height="796" alt="23_ssh_sudo_member" src="https://github.com/user-attachments/assets/d9b70d51-4ac2-4ec9-a23e-21236bdca99d" />
- File permission and ownership validation <img width="1009" height="787" alt="24_ssh_file_permisions" src="https://github.com/user-attachments/assets/8f445a43-39b1-49d4-a4af-fee705944c65" />

---

# 🚀 Skills Demonstrated Across Lab 5

- Linux CLI administration  
- User and group management  
- Sudo privilege delegation  
- File permission control  
- SSH remote management  
- Cross-platform troubleshooting  

---

---

# 🧪 Lab 6 – Security Hardening & Vulnerability Management  
*(Endpoint Protection & Risk Remediation)*

## 🔎 Overview

In this lab, I implemented endpoint hardening measures across Windows and Linux systems and performed vulnerability scanning to identify and remediate security risks within the domain environment.

This demonstrates:

- Endpoint protection management
- Firewall configuration
- Vulnerability assessment
- Patch management fundamentals
- Security-focused troubleshooting

---

## 🛡 Windows Endpoint Hardening

### Verified Security Controls

- Confirmed Windows Defender real-time protection enabled
- Reviewed security intelligence updates
- Validated system health status

### Windows Firewall Configuration

- Reviewed inbound and outbound rules
- Blocked inbound Remote Desktop (RDP)
- Verified RDP connection failure after rule implementation

---

### 🎯 Why This Matters

Demonstrates:

- Secure baseline configuration
- Reduction of attack surface
- Understanding of remote access security risks

---

## 🔍 Vulnerability Scanning

Installed OpenVAS on Ubuntu Server and conducted internal vulnerability scans against:

- Domain Controller (DC01)
- Windows 11 Workstation

### Process

1. Configured scan targets
2. Initiated vulnerability scan
3. Reviewed findings
4. Identified misconfigurations or outdated software
5. Applied remediation through:
   - Windows Updates
   - Configuration adjustments
   - Service hardening

---

### 🎯 Why This Matters

Bridges cybersecurity and IT operations by demonstrating:

- Vulnerability identification
- Risk assessment
- Remediation workflow
- Practical security implementation

---

## 🔄 Patch Management & Remediation

- Applied Windows Updates to domain systems
- Verified update installation
- Re-ran vulnerability scan to confirm remediation improvements

---

## 📸 Evidence

- `25_defender_dashboard.png` – Windows Defender status dashboard  
- `26_RDP_blocked.png` – Firewall rule blocking RDP  
- `27_openVAS_scans.png` – OpenVAS vulnerability scan results  

---

### 🚀 Skills Demonstrated

- Windows Defender management  
- Firewall rule configuration  
- Vulnerability scanning (OpenVAS)  
- Risk remediation  
- Patch management  
- Security hardening practices  

---

# 🏁 Enterprise Lab Summary

This repository demonstrates hands-on experience with:

- Active Directory administration  
- Group Policy implementation  
- Role-Based Access Control (RBAC)  
- File server & NTFS permissions  
- DNS & DHCP configuration  
- Linux system administration  
- Vulnerability management  
- Endpoint security hardening  
- Cross-platform troubleshooting  

This lab environment simulates real-world enterprise IT infrastructure and support workflows.










