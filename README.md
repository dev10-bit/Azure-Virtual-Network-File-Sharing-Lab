# azure-it-support-lab
 Hands-on Azure project simulating a basic IT support lab environment using VMs, networking, and file sharing.
# Azure IT Support Lab 🖥️☁️

This project simulates a real-world IT support environment using Microsoft Azure. It includes:

- 1 Windows Server VM
- 1 Windows 10 Client VM
- A virtual network with internal communication
- Shared folder access between VMs
- Network troubleshooting tools (ping, RDP, file sharing)

---

## 🔧 Project Goals

- Practice core Azure services
- Understand VM deployment and networking
- Configure secure file sharing
- Test internal connectivity using ping and mapped drives

---

## 🧱 Architecture

![architecture diagram] (screenshots/architecture.png.png)

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
- Resource Group: `Azure-IT-Lab`
- Virtual Network: `IT-VNet` with subnet `10.0.0.0/24`

### 2. Virtual Machines
- **ServerVM:** Windows Server 2022, RDP enabled
- **ClientVM:** Windows 10 Pro, RDP enabled

### 3. Network Configuration
- Both VMs in the same subnet
- Inbound rule for ICMP (ping)
- Shared folder on `ServerVM` accessed from `ClientVM`

### 4. File Sharing Test
- Created `C:\SharedFiles` on `ServerVM`
- Shared with `Everyone` (read/write)
- Accessed from `ClientVM` via: `\\10.0.0.4\SharedFiles`
