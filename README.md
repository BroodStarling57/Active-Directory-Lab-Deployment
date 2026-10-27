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

## Topology:
<p align="left"> The diagram below illustrates the general topology that will be referenced throughout the project, highlighting the overall structure, components and relationships between the various elements involved. </p>

<p align="center"><img width="522" height="292" alt="image" src="https://github.com/user-attachments/assets/fe1aa328-83d7-41c0-9b38-1537fb1d931b" /> </p>

# Methodology:
Step 1: Download Oracle VirtualBox and relevant ISOs listed above. Create two virtual machines (names can be of any personal preference), one to act as the domain controller (DC) for AD administration and the other as a client machine. Install Windows Server 2022 ISO onto the DC and Windows 10 ISO to the client virtual machine (VM). 

<p align="center"> <img width="934" height="502" alt="image" src="https://github.com/user-attachments/assets/0d7e1d65-37fa-4d97-8c7c-66cb4b83aa40" /> </p>

<div align="left"> Step 2: After both VM’s have been created, right-click into the DC, Settings -> Expert -> Network; enable two adapters, configuring each to attach respectively to NAT and an internal network. Access the same settings in the client machine but only enable one adapter, configuring it to attach to the internal network.

<br>**Tip:** Before we start using these machines, go to Settings -> Expert -> General -> Features and enable bi-directional drag ‘n drop and shared clipboard for quality-of-life purposes.

Now that’s been completed, for the next few steps we will put aside the client machine and focus solely on configuring our DC VM.

Step 3: Start the DC machine and complete the entire installation process. When choosing the specific version, select ‘Windows Server 2022 standard version (desktop experience)’. Any non-desktop experience options will lead to technical issues/difficulties. Select custom install when asked and disable any optional settings to avoid excessive prompts during the project. When creating your first account credentials to access the desktop, insert a memorable password. This project used ‘Password1’ for all password prompts for simplicity sakes (avoid complex passwords as it will only impede project tasks and progress).

Step 4: From the desktop, go to the bottom right corner and hover over the network icon. Select ‘Network & Internet Settings’ -> Change Adapter Options. This displays the two network adapters linked to the DC. Rename them accordingly to differentiate between the two, this project renamed them ‘Internet’ and ‘Internal’. The method to distinguishing involves right-clicking each adapter, selecting ‘status’ and checking whether it has a private or APIPA address; the adapter allocated the private, functioning address is connected to the internet whilst the APIPA allocation corresponds to the internal connection. An APIPA is assigned since we have yet to allocate our chosen IP and default gateway for the device within our private network, however the next step shall explain how we shall do this.

Step 5: Right-click on the adapter corresponding to the internal network; Properties -> ‘Internet Protocol Version 4 (TCP/IPv4)’ -> Properties. This presents an interface that allows you to allocate your own IP address, subnet mask, default gateway and DNS. Refer to the topology and insert the aforementioned details. The default gateway is left empty as the DC machine will act as its own DNS server, configured with the necessary features and functions so it won’t require a gateway to reach any external server, hence a loopback address suffices to fill the ‘Preferred DNS Server’ field. </div>
