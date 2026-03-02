# 🏢 Enterprise Windows Domain Lab  
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

- <img width="1046" height="775" alt="01_ADUC_org" src="https://github.com/user-attachments/assets/c723d86f-02ac-4267-9bd9-7e438034e797" />
 – Full OU structure in Active Directory Users and Computers  
- `02_Kobe_in_IT_OU.png` – User located in IT OU  
- `03_computerOU.png` – Windows 11 computer object in Corp-Computers OU  
- `04_security_groups.png` – Security group configuration (Global + Security)  
- `05_ITadmin_members.png` – Group membership assignments  

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

- `06_baseline_security.png` – GPO linked at domain level  
- `07_accountpolicy.png` – Account lockout configuration  
- `08_password_policy.png` – Password policy settings  

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

- `09_user_restriction.png` – User Configuration GPO settings  
- `10_win11_no_control_panel.png` – Control Panel disabled on Windows 11  

---

## 👮 Local Administrator Control via Group Policy

To enforce **least privilege**, I implemented centralized local administrator management.

### 🛠 Steps Implemented

1. Created Security Group: `Workstation-LocalAdmins`
2. Nested `IT-Admins` inside `Workstation-LocalAdmins`
3. Configured GPO:

