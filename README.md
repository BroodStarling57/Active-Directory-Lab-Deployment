# Active Directory Deployment

<p align="left"> 
This project aims to assist in the configuration of an active directory (AD) environment, similar to those found in real-world corporate systems. A PowerShell script will be used to create fictional domain user accounts, replicating a realistic organisational structure whilst providing practical, hands-on experience with skills relevant to active directory administration. To facilitate this learning process, the following environment shall be deployed using virtualisation software, providing an isolated and controlled setting that both minimises the risk of any misconfigurations affecting the host operating system and promotes experimentation; flexibility and security is improved whilst simultaneously making this project more friendly to users with limited hardware capabilities. </p>

## Software Requirements 💻:
- 🔗 [Oracle VirtualBox](https://www.virtualbox.org/wiki/Downloads) 
- 🔗 [Windows Server 2022 Evaluation ISO](https://www.microsoft.com/en-us/evalcenter/download-windows-server-2022) 
- 🔗 [Windows 10 Evaluation ISO](https://www.microsoft.com/en-gb/software-download/windows10)

## Hardware Requirements 💾⚙️:
-	Minimum RAM: 2 GB per virtual machine (VM).
-	Recommended RAM: 4GB of usable RAM per VM.

**WARNING:** Ensure the combined RAM allocated to all VMs does not exceed 50% of your host system's total RAM, leaving sufficient memory for the host OS and other applications.

Personal RAM Utilisation for VM’s:
| **Domain Controller** | **4GB (4096MB)** |
|:---------------------:|:----------------:|
| **Client1**           | **2GB (2048MB)** | 

## Topology
<p align="left"> The diagram below illustrates the general topology that will be referenced throughout the project, highlighting the overall structure, components and relationships between the various elements involved. </p>

<p align="center"><img width="522" height="292" alt="image" src="https://github.com/user-attachments/assets/fe1aa328-83d7-41c0-9b38-1537fb1d931b" /> </p>
