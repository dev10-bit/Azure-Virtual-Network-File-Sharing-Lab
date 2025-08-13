# Azure File Share Between Two Azure Windows VMs

A hands-on project that deploys two Windows VMs in a single Azure VNet, secures them with NSGs/Windows Firewall, and configures a Windows file share on the Server VM that is mapped and used from the Client VM. Includes end-to-end connectivity testing (ICMP) and read/write validation both directions.

## Architecture

![Architecture: Two Windows VMs in one VNet with NSGs and SMB share](images/A_network_diagram_illustrates_an_Azure_cloud_envir.png)

---

# Installation & Demonstration (Step-by-Step)

> **How to read this section:**  
> Each step has a short instruction and one screenshot with a precise title. Follow the steps in order; you’ll end with verified two-way file sharing.

## 1) Azure setup

1. **Create a Resource Group**
   - *Azure Portal → Resource groups → Create.*
   - Use **Azure-IT-Lab**, Region **East US**.  
   ![Create Resource Group - Azure-IT-Lab (East US)](images/Screenshot_1.png)

2. **Create a Virtual Network (Basics)**
   - Name **IT-VNet**, RG **Azure-IT-Lab**, Region **East US**.  
   ![Create Virtual Network - Basics (IT-VNet, Azure-IT-Lab)](images/Screenshot_2.png)

3. **Define Address Space and Subnet**
   - Address space **10.0.0.0/16**; default subnet **10.0.0.0/24**.  
   ![Create Virtual Network - IP Address space 10.0.0.0/16, default subnet /24](images/Screenshot_3.png)

4. **Review + Create the VNet**
   - Confirm validation passed and deploy.  
   ![Create Virtual Network - Review + Create (Validation passed)](images/Screenshot_4.png)

5. **Verify VNet shows in the Resource Group**
   - RG overview lists **IT-VNet**.  
   ![Resource Group Overview - IT-VNet deployed](images/Screenshot_5.png)

6. **Open the VNet Overview**
   - Address space **10.0.0.0/16**, Azure-provided DNS.  
   ![IT-VNet Overview - Address space 10.0.0.0/16](images/Screenshot_6.png)

7. **Deploy Two Windows VMs (ServerVM & ClientVM)**
   - Both in **IT-VNet / default** subnet, Windows images, public IPs enabled.  
   - After deployment, confirm both are **Running**.  
   ![Virtual Machines list - ClientVM and ServerVM running](images/Screenshot_7.png)

8. **Capture VM Details**
   - **ClientVM:** private **10.0.0.5**, public **20.115.41.100**.  
   ![ClientVM Overview - Private IP 10.0.0.5, Public IP 20.115.41.100](images/Screenshot_8.png)
   - **ServerVM:** private **10.0.0.4**, public **20.51.216.86**.  
   ![ServerVM Overview - Private IP 10.0.0.4, Public IP 20.51.216.86](images/Screenshot_9.png)

9. **Open NSG and Allow Required Inbound Traffic**
   - Ensure **RDP (TCP/3389)** allowed for management.
   - Add/verify **ICMP (AllowPing)** for lab ping tests.
   - (Shown here on **ServerVM** NSG.)  
   ![ServerVM NSG - AllowPing (ICMP) and RDP 3389 inbound](images/Screenshot_10.png)

> *(Optional reference of subnets blade used later)*  
> ![IT-VNet - Subnets blade showing default /24](images/Screenshot_43.png)

---

## 2) Configure Windows file sharing on the **ServerVM**

10. **RDP into ServerVM**
    - From your PC: `mstsc` → connect to ServerVM public IP.  
    ![Launch RDP client (mstsc) from Run dialog](images/Screenshot_12.png)  
    ![RDP connection prepared to ServerVM public IP](images/Screenshot_13.png)

11. **Create the shared folder**
    - Create `C:\SharedFiles`.  
    ![ServerVM - C:\SharedFiles created (empty)](images/Screenshot_14.png)

12. **Enable sharing + permissions**
    - **Advanced Sharing → Share this folder** as **SharedFiles**.  
    - **Permissions → Everyone: Full Control** (lab simplicity).  
    ![ServerVM - Share Permissions for 'SharedFiles' (Everyone: Full Control)](images/Screenshot_15.png)  
    ![ServerVM - Advanced Sharing enabled for 'SharedFiles'](images/Screenshot_16.png)

13. **Create a test file on the Server**
    - In `C:\SharedFiles`, create **AzureLabTest.txt** with sample text.  
    ![ServerVM - Create AzureLabTest.txt in C:\SharedFiles](images/Screenshot_23.png)

> *(Windows Firewall ICMP rule validation—used for ping tests)*  
> ![Windows Defender Firewall - File and Printer Sharing (Echo Request - ICMPv4-In) enabled](images/Screenshot_41.png)

---

## 3) Map the share on the **ClientVM** & verify

14. **RDP into ClientVM**
    - From your PC: connect to ClientVM public IP.  
    ![RDP connection prepared to ClientVM public IP](images/Screenshot_17.png)

15. **Map a network drive to the Server share**
    - **This PC → Map network drive**
    - Drive letter **Z:**; Folder `\\10.0.0.4\SharedFiles`; **Reconnect at sign-in** checked.  
    ![ClientVM - Map Z: to \\10.0.0.4\SharedFiles](images/Screenshot_21.png)

16. **Confirm the mapped drive shows the server’s file**
    - `Z:\` displays **AzureLabTest.txt**.  
    ![ClientVM - Z: shows AzureLabTest.txt from Server](images/Screenshot_22.png)

17. **Open the file from the Client**
    - Read **AzureLabTest.txt** via mapped drive.  
    ![ClientVM - Open AzureLabTest.txt via Z: (content visible)](images/Screenshot_24.png)

---

## 4) Verify **Client → Server** write (two-way test)

18. **Create a file from the Client in the share**
    - In `Z:\content` (create folder if needed), create **ClientCreated.txt**.  
    ![ClientVM - Create ClientCreated.txt in Z:\content](images/Screenshot_34.png)

19. **Confirm the Server sees the new client file**
    - On ServerVM: `C:\SharedFiles\content` shows **ClientCreated.txt** present.  
    ![ServerVM - ClientCreated.txt appears in C:\SharedFiles\content](images/Screenshot_35.png)

---

## 5) Connectivity tests (ICMP across private IPs)

20. **(Before enabling ICMP) Ping failure (expected)**
    - From ServerVM → `Test-Connection 10.0.0.5` fails until ICMP allowed.  
    ![ServerVM PowerShell - Ping to 10.0.0.5 failed before ICMP rule](images/Screenshot_39.png)

21. **(After enabling ICMP) Ping success**
    - From ServerVM → `Test-Connection 10.0.0.5` succeeds (~1ms).  
    ![ServerVM PowerShell - Ping to 10.0.0.5 succeeds after ICMP rule](images/Screenshot_37.png)
    ![ServerVM PowerShell - Ping to 10.0.0.5 succeeds after ICMP rule](images/Screenshot40.png)



22. **Client → Server ping success**
    - From ClientVM → `Test-Connection 10.0.0.4` succeeds.  
    ![ClientVM PowerShell - Ping to 10.0.0.4 succeeds](images/Screenshot_42.png)

---

## Results

- Both VMs deployed in **IT-VNet** (10.0.0.0/16, default /24).  
- **Windows share** exposed from **ServerVM** at `\\10.0.0.4\SharedFiles`.  
- **ClientVM** successfully **mapped Z:** and **read** Server’s file.  
- **ClientVM** created a file in the share; it **appeared on ServerVM** (write verified).  
- **ICMP** working both directions after enabling NSG + host firewall rules.

---

## What you’ll need

- Azure subscription, Owner/Contributor rights for RG.  
- RDP access to both public IPs (lab only).  
- Windows user creds for each VM.

---

## Clean-up

- Unmap the drive on ClientVM.  
- Stop/Delete the two VMs, NICs, public IPs, and the RG to avoid charges.

---



