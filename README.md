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

- "whoami /groups"
- "net localgroup administrators"

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

### 🚀 Skills Demonstrated

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

Validated name resolution using:









