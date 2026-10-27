# Active Directory Deployment

<p align="left"> 
This project aims to assist in the configuration of an active directory (AD) environment, similar to those found in real-world corporate systems. A PowerShell script will be used to create fictional domain user accounts, replicating a realistic organisational structure whilst providing practical, hands-on experience with skills relevant to active directory administration. To facilitate this learning process, the following environment shall be deployed using virtualisation software, providing an isolated and controlled setting that both minimises the risk of any misconfigurations affecting the host operating system and promotes experimentation; flexibility and security is improved whilst simultaneously making this project more friendly to users with limited hardware capabilities. </p>

### Software Requirements 💻:
- 🔗 [Oracle VirtualBox](https://www.virtualbox.org/wiki/Downloads) 
- 🔗 [Windows Server 2022 Evaluation ISO](https://www.microsoft.com/en-us/evalcenter/download-windows-server-2022) 
- 🔗 [Windows 10 Evaluation ISO](https://www.microsoft.com/en-gb/software-download/windows10)

### Hardware Requirements 💾⚙️:
-	Minimum RAM: 2 GB per virtual machine (VM).
-	Recommended RAM: 4GB of usable RAM per VM.

**WARNING:** Ensure the combined RAM allocated to all VMs does not exceed 50% of your host system's total RAM, leaving sufficient memory for the host OS and other applications.

Personal RAM Utilisation for VM’s:
| **Domain Controller** | **4GB (4096MB)** |
|:---------------------:|:----------------:|
| **Client1**           | **2GB (2048MB)** | 

## AD Topology
<p align="left"> The diagram below illustrates the general topology that will be referenced throughout the project, highlighting the overall structure, components and relationships between the various elements involved. </p>

<p align="center"><img width="1919" height="1079" alt="#1" src="https://github.com/user-attachments/assets/2ed7d17b-4404-43bd-8b61-503fda309ce9" /></p>

# Methodology
Step 1: Download Oracle VirtualBox and relevant ISOs listed above. Create two virtual machines (names can be of any personal preference), one to act as the domain controller (DC) for AD administration and the other as a client machine. Install Windows Server 2022 ISO onto the DC and Windows 10 ISO to the client virtual machine (VM). 

<p align="center"> <img width="934" height="502" alt="image" src="https://github.com/user-attachments/assets/0d7e1d65-37fa-4d97-8c7c-66cb4b83aa40" /> </p>

<div align="left"> Step 2: After both VM’s have been created, right-click into the DC, Settings -> Expert -> Network; enable two adapters, configuring each to attach respectively to NAT and an internal network. Access the same settings in the client machine but only enable one adapter, configuring it to attach to the internal network.

<br>**Tip:** Before we start using these machines, go to Settings -> Expert -> General -> Features and enable bi-directional drag ‘n drop and shared clipboard for quality-of-life purposes.

Now that’s been completed, for the next few steps we will put aside the client machine and focus solely on configuring our DC VM.

Step 3: Start the DC machine and complete the entire installation process. When choosing the specific version, select ‘Windows Server 2022 standard version (desktop experience)’. Any non-desktop experience options will lead to technical issues/difficulties. Select custom install when asked and disable any optional settings to avoid excessive prompts during the project. When creating your first account credentials to access the desktop, insert a memorable password. This project used ‘Password1’ for all password prompts for simplicity sakes (avoid complex passwords as it will only impede project tasks and progress).

Step 4: From the desktop, go to the bottom right corner and hover over the network icon. Select ‘Network & Internet Settings’ -> Change Adapter Options. This displays the two network adapters linked to the DC. Rename them accordingly to differentiate between the two, this project renamed them ‘Internet’ and ‘Internal’. The method to distinguishing involves right-clicking each adapter, selecting ‘status’ and checking whether it has a private or APIPA address; the adapter allocated the private, functioning address is connected to the internet whilst the APIPA allocation corresponds to the internal connection. An APIPA is assigned since we have yet to allocate our chosen IP and default gateway for the device within our private network, however the next step shall explain how we shall do this.

Step 5: Right-click on the adapter corresponding to the internal network; Properties -> ‘Internet Protocol Version 4 (TCP/IPv4)’ -> Properties. This presents an interface that allows you to allocate your own IP address, subnet mask, default gateway and DNS. Refer to the topology and insert the aforementioned details. The default gateway is left empty as the DC machine will act as its own DNS server, configured with the necessary features and functions so it won’t require a gateway to reach any external server, hence a loopback address suffices to fill the ‘Preferred DNS Server’ field. 

See the image below for reference as to how things should look. </div>

<p align="center"> <img width="940" height="468" alt="image" src="https://github.com/user-attachments/assets/bca01b87-a87e-4eb6-bd70-7e9a35ee16fb" /> </p>

<br><div align="left">**TIP:** Before the completing the next step, on the interactive menu at the top of the VM, select ‘Devices’ and ‘Insert Guest Additions CD Image’. Then File explorer -> This PC -> ‘CD Drive (D:) VirtualBox Guest Additions’ -> Click and Install ‘VBox WindowsAdditions-amd64’. Once installed, choose to restart manually at a later time. This allows you to resize the VM window as you wish. 

(Optional Step: Go to Settings -> System -> Rename PC; purely namesake for identification. Choose to restart now or manually later).

We will now begin adding the necessary roles and features we want a part of our active directory, referencing the bottom part of our topology, this would include; domain services, NAT/RAS and DHCP server scope. </div>

**Note:** The following steps 6–8 can be installed together to save time as shown below. However, during the initial learning process, it is best advised you follow steps in order to understand each feature and its installation.

<p align="center"> <img width="601" height="299" alt="image" src="https://github.com/user-attachments/assets/61d2bfb6-c57f-453f-8aee-0545fd93df38" /> </p>

Step 6: Add roles & features -> Add ‘Active Directory Domain Services’ -> Post-Deployment Configuration (You will see a flag in the top-right corner of the server manager application with a warning icon, click on it and select the prompt for configuration). A window will open similar to what is displayed below, input the name of your domain, click next. Input your chosen password when requested and then click next till you are able to install. After this process is completed, the device will restart as a virtual domain controller, otherwise known as an administrator.

<div align="center"><img width="601" height="299" alt="image" src="https://github.com/user-attachments/assets/0aed0d50-7c02-4589-8b02-5dc0ae0d5308" />

<img width="940" height="467" alt="image" src="https://github.com/user-attachments/assets/a4096c9b-924a-4d3a-afd2-27e09b805a02" /> </div>

Step 7: Log back into the domain controller and open server manager. Add roles & features -> Add ‘Remote Access’; when on the role services tab, be sure to tick the routing feature option. On the top-right corner of server manager, go to Tools -> Remote Access & Routing. Right-click ‘DC(local)’ and select ‘Configure Routing and Remote Access’ (The former’s name will change depending on what you have named the DC VM). Once the window opens select NAT type and then ‘Use public interface to connect to the internet’. Click next until confirm/install and close window. Right-click server status once again and refresh, you will see that it becomes green to validate the change that has been implemented.

**Note:** If the ‘Use public interface to connect to the internet’ option is unavailable or unselectable, close the window and repeat the process till it does, this bug is commonly faced when handling this tool, it is nothing to worry about.

Step 8: To implement the DHCP server for our active directory; Add roles & features -> DHCP Server. Click next till you reach the end and install. After installation is complete, open tools in server manager and select DHCP. Expand the tab denoting the name of your domain (this project personally used ‘mydomain.com’ so the tab would be labelled ‘dc.mydomain.com’), right-click IPv4 and select ‘New Scope’. Refer to the topology and input the necessary details. The name of the scope is upto personal discretion, this project simply named it ‘172.16.0.100-200’ (if you forget to input a default gateway, Server Options -> 003 Routing -> Input IP). Authorise the DHCP server following this and refresh IPv4/6.

All roles and features necessary for our active directory to function have now been configured.

Before we begin creating simulated users for our directory, we will first create a dedicated admin account for our active directory to add an aspect of security within this project. This can be done prior to our feature installations, but for simplicity’s sake we will do it now. When repeating this project, play around and jumble steps around to find out what configuration process you prefer.

Step 9: Start -> ‘Administrative Tools’ -> ‘Active Directory Users and Computers’. Right-click your domain name tab and create an organisational unit called ‘Admins’. Create a new user within this folder following the admin account convention ‘a-(First initial)(Last name)’. Make sure password is memorable and there is no expiry date. After credentials have been created, right-click the user account and access properties; click ‘member of’, find and resolve to ‘Domain Admins’, then click apply. Log out and into the dedicated account, which we will use for the remainder of this project. The images below can be used as references as to how things should look.

<div align="center"><img width="601" height="299" alt="image" src="https://github.com/user-attachments/assets/e3550405-f879-424c-bc07-5f86fe2c9c89" />
<img width="601" height="299" alt="image" src="https://github.com/user-attachments/assets/e9384b35-569a-4261-822c-f33caac242d0" /> </div>

**Tip:** Server Manager -> Local Server -> IE Enhanced Security Configuration, Turn OFF. It reduces the amount of security when running unknown scripts and websites, making the overall process much smoother.

Step 10: To begin creating our simulated users, download the script and text file attached to the repository, copy-pasting both files as a folder onto the desktop. Open the text file and write your name into it at the top of the list so a regular domain account corresponding to you is created. Start -> Windows PowerShell -> Windows PowerShell ISE, run this programme as an administrator to ensure commands function. Click ‘Open Script’ on the toolbar and select script on desktop. Remove existing security features using ‘Set-ExecutionPolicy Unrestricted’, navigate to directory holding the script using the ‘cd’ command (Use the ‘ls’ command to verify contents inside). Once that is done, run the script and a period of time will be taken to create the approximate 1k users. 

<p align="center"><img width="602" height="299" alt="image" src="https://github.com/user-attachments/assets/fddea346-782a-4a28-a4f4-a371262565b9" /></p>

**Tip:** For proof purposes, as showcased in the image above, head back to ‘Active Directory Users & Computers’ and it will display all the newly added users to the virtual domain.

Step 11: If the client machine has already been created according to prior instructions, start it alongside the DC so that both machines are running simultaneously. Once done, a notification will appear on the client machine desktop asking whether the device should be discoverable to other networks, click yes. On the client VM, Settings -> Rename this PC (advanced) -> Change.  Rename the computer (this project renamed the client machine to ‘Client1’) and join the domain, use the image below as reference.

<p align="center"> <img width="601" height="299" alt="image" src="https://github.com/user-attachments/assets/bac0ff11-587a-4af5-be72-d0b78bd90efe" /></p>

<p align="center"> Once this setting is applied, the active directory environment has successfully been created. </p>

<p align="center"> <img width="601" height="324" alt="image" src="https://github.com/user-attachments/assets/961c800b-4239-42dc-a565-67e4316aee4e" /> </p>

Step 12: Congratulations! You’ve reached the end of the project as there’s nothing more to do. Your active directory home lab is now set up and ready for you to use. Enjoy exploring its features, experimenting with different configurations, and having fun learning in your new lab environment.

## Conclusion
This documented project provided me an understanding of the processes involved in deploying a basic active directory environment, establishing a foundation for developing my practical skills and gaining experience in system administration. It is recommended that the created lab environment be extended by implementing group policies to emulate an organisational structure, alongside further enhancements such as organising users into organisational units (OUs) whilst configuring appropriate access controls and permissions. These additions outside the scope of the project, provide opportunities to build on the existing environment and develop a deeper understanding of active directory management, security, and automation.

## Reference
▶️ [How to Setup a Basic Home Lab Running Active Directory](https://www.youtube.com/watch?v=MHsI8hJmggI&t=5s)
