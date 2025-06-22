# Azure IT Support Lab 🖥️☁️

Hands-on Azure project simulating a basic IT support lab environment using VMs, networking, and file sharing.

---

## 🔧 Project Goals
- Practice core Azure services  
- Understand VM deployment and networking  
- Configure secure file sharing  
- Test internal connectivity using ping and mapped drives  

---

## 🧱 Architecture
![Architecture Diagram](screenshots/architecture.png)

---

## 🛠️ Technologies Used
- Microsoft Azure  
- Windows Server 2022  
- Windows 10 Pro  
- PowerShell  
- Remote Desktop (RDP)  
- Shared Network Drive (SMB)

---

## 🔄 Lab Setup Overview

### 1. Azure Resources
- **Resource Group:** Azure-IT-Lab  
- **Virtual Network:** IT-VNet (Subnet: 10.0.0.0/24)

### 2. Virtual Machines
- **ServerVM:** Windows Server 2022, IP `10.0.0.4`, RDP enabled  
- **ClientVM:** Windows 10 Pro, IP `10.0.0.5`, RDP enabled  

### 3. Network Configuration
- Both VMs reside in the same subnet  
- Inbound security rule allows **ICMP (ping)**  
- Verified connectivity via PowerShell ping from ServerVM to ClientVM  

---

## 📁 File Sharing Test

### Folder Setup on ServerVM
- Created folder: `C:\SharedFiles\content`  
- Shared with **Everyone (read/write)**

### Files Created
- `testnote.txt`  
- `githublab.txt`  

### PowerShell Commands Used:
```powershell
Set-Content -Path "C:\SharedFiles\testnote.txt" -Value "This is a test note created from PowerShell."
Set-Content -Path "C:\SharedFiles\githublab.txt" -Value "Arsenal FC."
