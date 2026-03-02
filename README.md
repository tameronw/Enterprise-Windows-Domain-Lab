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

