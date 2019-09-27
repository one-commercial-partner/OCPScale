# Azure Backup Server

In this lab you will  prepare your environment to back up workloads using Microsoft Azure Backup Server (MABS). With Azure Backup Server, you can protect application workloads such as Hyper-V VMs, Microsoft SQL Server, SharePoint Server, Microsoft Exchange, and Windows clients from a single console.

MABS deployed in an Azure VM can backup VM's in Azure but they should be in same domain to enable backup operation. The process to back an Azure VM remains same as backing up VMs on premises, however deploying MABS in Azure has some limitations. For more information on limitation see [DPM as an Azure virtual machine](https://docs.microsoft.com/system-center/dpm/install-dpm?view=sc-dpm-1807#setup-prerequisites).

Azure Backup Server inherits much of the workload backup functionality from Data Protection Manager (DPM). Though Azure Backup Server shares much of the same functionality as DPM, Azure Backup Server does not back up to tape, nor does it integrate with System Center.

When choosing a server for running Azure Backup Server, it is recommended you start with a gallery image of Windows Server 2012 R2 Datacenter, Windows Server 2016 Datacenter or Windows Server 2019 Datacenter.  The recommended minimum requirements for the server virtual machine (VM) should be: A2 Standard with two cores and 3.5 GB RAM.

Protecting workloads with Azure Backup Server has many nuances. The article, [Install DPM as an Azure virtual machine](https://technet.microsoft.com/library/jj852163.aspx), helps explain these nuances. Before deploying the machine, read this article completely.

## Setup an IaaS Domain Controller via JSON Template
We will setup an IaaS VM with Active Directory via a JSON template from GitHub.  Although this domain controller is the in the cloud, we’ll use it to simulate an on-prem domain controller.

*Please note this task will take approximately **30 minutes** to complete.*
### Task 1 - Install the domain controller
1.	Logon to your Azure subscription.
2.	Surf to https://azure.microsoft.com/en-us/resources/templates/active-directory-new-domain/ 
3.	Select **Deploy to Azure**. 
4.	Enter the following information:
    * Resource Group: *Create New*: **AZDC**
    * Location: Pick a supported location
    * Admin Username: **yourname** *you should write this down*
    * Admin Password: `Complex.Password` *you should write this down*
    * Domain name:  Enter a FQDN such as mydomainname.com and keep the name shorter than 15 characters (that’s a NetBIOS restriction) *you should write this down*
    * DNS Prefix: use the letter “a” and then the last four digits of your cell phone, *e.g. a1234*
5.	Scroll down, click **I agree to the terms and conditions stated above** and then **Purchase**.  Monitor the deployment by clicking on the “Deploying Template deployment” tile within the Azure Portal.
    * Confirm that you don’t have any validation errors.  If you do, correct them before moving forward. 
    * If the deployment fails, examine the logs to see what the root cause is.
    * You’ll need to delete the Resource Group before you try running the template again. 
    * If the template takes you back to the Microsoft Azure portal and the deployment begins, monitor the status for any errors.
6.	The deployment and build of the VM will take upwards of 30 minutes depending on several factors.  Don’t forget that we’re not only spinning up a VM but we are also installing and configuring DNS and running DCPromo.  

## Setup a VM for MABS
MABS needs to run on it's own dedicated VM.
### Task 2 - Build MABS VM
1.	Select **+ Create a resource** found on the upper, left corner of the Azure portal.
2.	Select **Windows Server 2016 Datacenter**.
3.	Enter, or select, the following information, accept the defaults for the remaining settings:
    * Resource Group: *Create New*: **MABS**
    * Virtual Machine Name: **MABS**
    * Region: **East US**
    * Size: *Change size:* **Standard Ds3_v2**
    * Username: enter a username and write it down
    * Password: `Complex.Password`
    * Confirm Password: `Complex.Password`
    * Public inbound ports:  **Open RDP**
    * Select **Next: Disks >**
4.	Click **Create and attach a new disk**, enter the following, and click **OK**:
    * Size: 4096
5. Click **Next: Networking >** and then enter the following:
    * Virtual Network: **adVNET**
    * Click **Next: Management >**
6.	Select the existing/previously created **Diagnostic storage account** and then click **Next: Advanced >**.
7.	Review the items and then click **Next: Tags >**.
8.	Review the items and then click **Next: Review + create**.
9.	Once validation passes click **Create**.

## Join the MABS VM to the domain
### Task 3 - Check the DNS Setting
1.	Connect to the **MABS** VM and logon. **Home/Virtual machines/MABS/Connect.**
2.	If prompted, click **No** on the Network discovery blade.
3.	Depending on which region you chose for setup, the **MABS**  VM may or may not have the DNS server set to a value we need.
4.	Open a **Command prompt** and enter `ipconfig /all | more`.
6.	If the DNS Server is set to 10.0.0.4, close the Command Prompt window and continue to the  **Task 5 - Join the Domain.**

### Task 4 - Configure DNS
1.	Within **Server Manager**, click on **Local Server**.
2.	Click on **IPv4 address assigned by DHCP, IPv6 enabled setting** for the Ethernet connection.
3.	Right-click on the network adapter and choose **Properties**.
4.	Select **Internet Protocol Version 4 (TCP/IPv4)** and then click **Properties**.
5.	Select the radio button for **Use the following DNS Server addresses:** and Set the DNS server to **10.0.0.4** and click **OK** and then **Close**.
6.	You will lose connection to the VM, this is expected. Once you are back at the Microsoft Azure Portal, click **Restart** to restart the **MABS**VM.
7.	Once the VM is successfully restarted, connect to the **MABS** VM and logon.

### Task 5 - Join the Domain
1.	Within **Server Manager**, click on **Local Server**.
2.	Click on **WORKGROUP**, then **Change** to rename this computer or join it to a domain.
3.	Click the radio button for **Domain**, enter your fully-qualified domain name, such as mydomainname.com, and click **OK**.
4.	In the Windows Security box enter the AD Domain Admin credentials you specified in the template.
5.	Click **Ok** on the Welcome screen, **Ok** on the Computer Name/Domain Changes window, **Close**, then **Restart Now**.

### Task 6 - Install the required MABS Components
1. Once the MABS VM is restarted complete the following:
    * RDP into the desktop and logon with your **domain credentials**, not your *MABS\username* account.
    * Click the **Windows Start** button,  then select **Control Panel**.
2. Under **Programs** select **Turns Windows Features on and off**.
3. Click **Next** four times and then select the **.NET Framework Features** check box, followed by selecting **Next** and then **Install**.
4. Click **Close** when complete.
5. Open an administrative PowerShell window and enter this command: **Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Hyper-V-Manage**

### Task 7 - Create a MABS Service Account
We are going to create a service account that MABS will use.
1.	Connect to the adVM virtual machine and logon with your domain account by selecting **Microsoft Azure / Resource Groups / AZDCRG / adVM / Connect**.  
2.	Click on **Download RDP File**.
3.	Logon with the fully qualified credentials you wrote down earlier (e.g. yourname@yourdomain.com).  You may have to choose __More Choices__ then **Use a different account** to enter your new set of credentials.
4.	If prompted, click **No** on the Network Discovery blade.
5.	Within Server Manager, click **Tools** and then **Active Directory Users and Computers**.
6.	Expand the tree and select the **Users Container**.
7.	On the toolbar click the icon to create a new user in the current container.  
8.	Create a New User with the following information:
    * First Name: **MABS**
    * Last Name: **Accounts**
    * Full Name: **MABSAccount**
    * User Logon Name: **mabsacct**
9.	Click **Next** and set the password to `Complex.Password`. Uncheck **User must change password at next logon**, and set the **Password never expires** checkbox.
10.	Click **Next** then **Finish**.
11.	Minimize the RDP window.

### Task 8 - Setup MABS
1.	Once the required components are installed, complete the following:
    * Disable IE ESC in Server Manager
    * Logon to the Azure Portal https://portal.azure.com
    * Select your Recovery Services Vault called `MyVault`  by clicking on the search box at the top of the portal.
2.	In the **Getting Started** section, click **Backup**.
3.	From the **Where is your workload running?** drop-down menu, select **On-premises**.
	*You chose On-premises because your Windows Server or Windows computer is a physical or virtual machine that is not in Azure.*
4.	From the What do you want to backup? menu, select **Hyper-V Virtual Machines**, and click **Prepare Infrastructure**.
5.	Click on **Download** for Microsoft Azure Server Backup and then **Download** again from within Internet Explorer. Select all items, click **Next**, and then **Save** all items.  You will need to allow pop-ups for this operation to complete. Open **View Downloads** to check progress.
6. Once the download process completes, run **System_Center_Microsoft_Azure_Backup_Server_v3** to begin installing Microsoft Azure Backup Server. Click **Next**, **Next**, then **Extract**. Click **Finish** when complete.
7.	Switch back to the Azure Portal and check the box for **Already downloaded or using the latest Azure Backup Server Installation**, and then click the **Download** button.  Save the credentials to your DOwnloads folder.
8. Open File Explorer and browse to `C:\System Center Microsoft Azure Backup Server v3` and then double-click on `setup.exe`. Under **Install** click **Microsoft Azure Backup Server** to launch the setup wizard.  It may takes several moments to launch as required components are installed in the background.
9.	On the **Welcome** screen click the **Next** button. This takes you to the Prerequisite Checks section. On this screen, click **Check** to determine if the hardware and software prerequisites for Azure Backup Server have been met. If all prerequisites are met successfully, you will see a message indicating that the machine meets the requirements. Click on the **Next** button.
10. Microsoft Azure Backup Server requires SQL Server Enterprise. Further, the Azure Backup Server installation package comes bundled with the appropriate SQL Server binaries needed if you do not wish to use your own SQL. When starting with a new Azure Backup Server installation, you should pick the option **Install new Instance of SQL Server with this Setup** and click the **Check and Install** button. Once the prerequisites are successfully installed, click **Next**.
11. On the **SQL Settings** screen click **Next**.
12. Provide a strong password such as `Complex.Password` on the **Security Settings** screen and click **Next**.
13. Select **Use Microsoft Update ...** and click **Next**.
14. Review the Summary of Settings and click **Install**.
15. The installation happens in phases. In the first phase the Microsoft Azure Recovery Services Agent is installed on the server. The wizard also checks for Internet connectivity. CLick **Next** on the **Proxy Configuration** screen.
16. Click **Install** on the **Installation** screen, and then **Next** to register this server with the vault.
17. On the Vault Identification screen complete the following steps:
    * Click **Browse**, find the credentials file you saved earlier, click **Open** and then **Next**. 
18. Generate a passphrase and save it to the Desktop folder by clicking **Browse** and then select **Desktop** under **This PC**.  Click **Next**.
19. The **Microsoft Update Opt-In** will process and several steps will complete.

*Please note that it may take upwards of 30 minutes to install the required components.*

When the installation step has completed, the product's desktop icons will have been created as well. Just double-click the icon to launch the product.

Notice the final status message:

*Data Protection Manager installed successfully.  You need to restart your computer to incorporate the changes made by DPM Setup.*

DPM and Azure Backup server share the same code base, with the only difference being:
1. MABS does not require a SYstem Center License
2. MABS does not support local tape drives
3. MABS requires an Azure subscription.

Restart the server once setup is complete.


## Task 9 - Open Windows Firewall
We need to allow communications between our source VM (ADVM) and the MABS Server.  Under normal circumstances you would open the appropriate ports, but for this lab we are simply disabling the firewall.
1. Connect to ADVM via RDP.
2. In Server Manager choose **Local Server**.
3. Click on Windows Firewall **DOmain:On**.
4. Select **Turn Windows Firewall on or off**.
5. Turn off Windows Firell for all networks and click **OK**.


## Task 10 - Install an agent
Once your MABS server reboots, logon and complete the following steps.

1. Click on the **Microsoft Azure Backup Server** icon from the desktop.
2. Click on **Management** then **Add**.  
3. On the Select Production Server type screen select **Windows Servers** and click **Next**.
4. Select Install agents and click Next.
5. On the **Select Computer** screen select **ADVM**, then **Add**, then **Next**.
6. Enter your credentials and click **Next**.
7. Select **Yes**, click **Next** then **Install**.
8. Monitor the progress and then click **Close**

## Task 11 - Add Disks
Your MABS Server needs local storage to save backup files.  Please complete the following steps to allocate storage.

1. **Right-click** the Windows Start button and click **Disk Management**.
2. Click **OK** to initialize disk 2.
3. Right-click **Disk 2** and choose **New Simple Volume**. Hit Next four times and then CLose.
4. From the MABS UI, click on Disk Storage, then Add.
5. Select and Add the E:\ volume and click **OK**.


[Back](index.md)