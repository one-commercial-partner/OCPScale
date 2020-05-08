# Azure Backup 

In this lab you are going to backup files and folders from your local computer to Microsoft Azure. This includes installing an agent on your computer.  If this is not possible, you can spin up a VM in Microsoft Azure to use as the source computer. 

## Create a recovery services vault 

To back up your files and folders, you need to create a Recovery Services vault in the region where you want to store the data.  

On the Hub menu, click More services and in the list of resources, type Recovery Services and click Recovery Services vaults. 

On the Recovery Services vaults menu, click Add. 

The Recovery Services vault blade opens, enter the following: 

Name: EastDRVault 

Subscription: Azure Pass 

Resource group: Create New DRaaSEast 

Location: East US 

At the bottom of the Recovery Services vault blade, click Create. 

It can take several minutes for the Recovery Services vault to be created. Monitor the status notifications in the upper right-hand area of the portal. Once your vault is created, it appears in the list of Recovery Services vaults. If after several minutes you don't see your vault, click Refresh. 

Page Break
 

Configure the vault 

On the Recovery Services vault blade in the Getting Started section, click Backup, then on the Getting Started with Backup blade, select Backup goal. 

From the Where is your workload running? drop-down menu, select On-Premises. 

From the What do you want to backup? menu, select Files and folders, and click Prepare infrastructure. 

On the Prepare infrastructure blade, click Download Agent for Windows Server or Windows Client.  A pop-up menu prompts you to run or save MARSAgentInstaller.exe. In the download pop-up menu, click Save. 

You don't need to install the agent yet. You can install the agent after you have downloaded the vault credentials. 

On the Prepare infrastructure blade, click the checkbox for Already downloaded or using the latest Recovery Services Agent and then click Download. 

The vault credentials download to your Downloads folder. After the vault credentials finish downloading, you see a pop-up asking if you want to open or save the credentials. Click Save. If you accidentally click Open, let the dialog that attempts to open the vault credentials, fail. You cannot open the vault credentials. Proceed to the next step. The vault credentials are in the Downloads folder. 

 

Install and register the agent 

Locate and double-click the MARSagentinstaller.exe from the Downloads folder (or other saved location). 

Complete the Microsoft Azure Recovery Services Agent Setup Wizard.  The setup may take 10 minutes if required components need to be installed. 

Note: 

If you lose or forget the passphrase, Microsoft cannot help recover the backup data. Save the file in a secure location. It is required to restore a backup. 

The agent is now installed and your machine is registered to the vault. You're ready to configure and schedule your backup. 

Page Break
 

Back up your files and folders 

The initial backup includes key tasks: 

Initialize the agent 

Schedule the backup 

Back up files and folders for the first time 

To complete the initial backup: 

Open Microsoft Azure Backup from your desktop.  It may take several minutes to initialize. 

Click on Register Server. 

On the Proxy Configuration screen click Next. 

On the Vault Credential screen click Browse to find your vault credentials.  Note that it may take several minutes to validate the Vault credentials. Click Next. 

On the Encryption Setting screen, click Generate Passphrase and save the passphrase to your downloads folder. Click Register, then click Close. (if you get an error here, proceed ignoring the Encryption error) 

Click Schedule Backup. 

Click Next, then Add items.  Add your documents folder and click Next. 

Click Next three times, then Finish, then Close. 

On the Actions menu choose Back Up Now, click Next and then Back Up. 

Congratulations!  You are now performing a Cloud First Backup! 

Recover Your Data 

In this lab you will restore an innocuous file you just backup up to Azure. 

Once you see that the Back Up job has completed, In the Azure Backup Agent select Recover Data. 

On the Getting Started screen choose this server and click Next. 

On the Select Recovery Mode screen select Individual files and folders and click Next. 

On the Select Volume and Date screen select C:\ and then click Mount. 

Click on Browse and notice File Explorer will open.  At this point you would copy the file or folder you want to recover over to your computer. 

Click Unmount. 

Page Break
 

Azure to Azure Site Recovery with ASR 

In this lab you will create a VM in Azure, install IIS, enable replications, and with time permitting failover the VM to a different Azure region. 

Before you Begin 

If you are using a Microsoft Azure subscription that was provided to you by Microsoft, you are limited to a specific set of Microsoft Azure regions that you can use. Please use either the East US, South Central US, West Europe, Southeast Asia, West US 2, or West Central US locations. 

cid:image001.png@01D2EAB5.12DB7C80 

Otherwise you will receive the following error in the portal if you select an unsupported region and attempt to build anything in Microsoft Azure. 

Create a IaaS Web Server 

Build the VM 

Return to the Azure portal and click the New button (the Plus) found on the upper left-hand corner of the Azure portal. 

Select Compute from the New blade, then select Windows Server 2016 Datacenter. 

Fill out the virtual machine Basics form and click OK: 

Name: Webby 

VM disk type: HDD 

User name: WebAdmin 

Password: Complex.Password 

Subscription: Azure Pass 

Resource Group: Create New EastWebServers 

Location: East US 

On the Choose a size blade select a VM with the lowest cost and at least 1 CPU and 3.5GB RAM (the options will vary based upon your subscription type).  Click Select. 

On the Settings blade, select No under Use managed disks. Keep the other settings as defaults and click OK. 

On the Create page, click Create to start the virtual machine deployment. 

To monitor deployment status, click the “Deploying Windows Server 2016 Datacenter” tile. The VM can be found on the Azure portal dashboard, or by selecting Virtual Machines from the left-hand menu. It should take less than 10 minutes to spin up the VM. 

When the VM has been created, the status changes from Creating to Running. 

Install and Configure IIS 

Connect to the Webby VM and logon as webby\WebAdmin. 

When prompted, click No on the Networks blade. 

Within Server Manager, select Add Role and Features under Manage. 

Click Next three times. 

On the Select server roles screen, select Web Server (IIS), then Add Features, then Next four times. Click Install and then Close when installation completes. 

Click on Tools then Internet Information Servers (IIS) Manager. 

Expand Webby until you can select the Default Web Site.  Right click on Default Web Site and choose Explore. 

Start Notepad as an Admin 

Modify the iisstart.html file with notepad and change <title>IIS Windows Server</title> to <title>Webby</title>. Save and close the file. 

Modify the NSG 

Ports 80 443 are not allowed through the default Network Security Group.  You will need to open these ports. 

In the Azure Portal click Resource Groups then EastWebServers. 

Click on the Webby-nsg. 

Under Setting, select Inbound security rules then +Add. 

Enter the following and click OK: 

Destination port ranges: 80 

Protocol: TCP 

Name: HTTPIn 

Under Setting, select Outbound security rules then +Add. 

Enter the following and click OK: 

Destination port ranges: 80 

Protocol: TCP 

Name: HTTPOut 

Select Outbound security rules then +Add 

Click the Basic button if it is present 

Under Service Select HTTPS 

Change the Priority to 110 

 

Confirm Web Traffic 

In the Azure Portal click Resource Groups then EastWebServers. 

Click on the Webby VM and copy the Public IP address. 

With a browser surf to this public IP address to confirm you are hitting your IIS server and web traffic is coming through. The name in the browser tab should be Webby IIS Server. 

Create a vault 

Create the vault in an approved region, except the source region. 

On the Hub menu, click More services and in the list of resources, type Recovery Services and click Recovery Services vaults. 

On the Recovery Services vaults menu, click Add. 

The Recovery Services vault blade opens, enter the following: 

Name: WestDRVault 

Subscription: Azure Pass 

Resource group: Create New DRaaSWest 

Location: West US 2 

At the bottom of the Recovery Services vault blade, click Create. 

Enable replication 

Select the source 

In the Azure Portal click Resource Groups then DRaaSWest. 

Click on WestDRVault and then +Replicate. 

In Source, enter the following and Click OK: 

Source: Azure – PREVIEW 

Source location: East US 

Azure virtual machine deployment model: Resource Manager 

Source resource group: EastWebServers 

On the Select Virtual Machines Blade select Webby and click OK. 

On the Configure settings blade set the Target Location to West US 2 and click Create target resources. 

Once validation completes, click Enable Replication. 

Track Replication 

You can track replication status the following: 

In the Azure Portal under your recovery vault, choose Monitoring and Reports click Jobs > Site Recovery Jobs. Monitor the Site Recovery job.  

Depending on the size of the source VM you chose Error ID 150050 may occur (The Completed with information message).  The means the source VM size is not available in the target geo.  You may need to update the target VM's size prior to failover if desired.  

 

Once Enable Replication is complete, in the Azure Portal click Protected Items > Replicated Items to examine the synchronization status of the VM.  Hit Refresh to monitor replication status. It may take 15-30 minutes to replicate the VM, so now is an appropriate time to take a break. 

 

Until the VM is 100% synchronized and Protected, a test failover is not possible.  It may take several hours for your VM to replicate in a production environment. 

Page Break
 

Run a Test Failover Disaster Recovery Drill 

Before you run a test failover, verify the VM properties to make sure everything's as expected. Access the VM properties in Replicated items. The Essentials blade shows information about machines settings and status. 

 

A test failover executes a failover but does not make the secondary VM active.  A drill validates your replication strategy without data loss or downtime and doesn't affect your production environment. 

Click the VM Test Failover icon. 

In Test Failover, select Latest (lowest RPO) as the recovery point to use for the failover.  Note the following: 

Latest (lowest RPO): Fails the VM over with the current state of the VM but requires some processing time. 

Latest processed (low RTO): Fails the VM over to the latest recovery point that was processed by the Site Recovery service. The time stamp is shown. With this option, no time is spent processing data, so it provides a low RTO (Recovery Time Objective) 

Latest app-consistent: This option fails over all VMs to the latest app-consistent recovery point. The time stamp is shown. 

Custom: Use this option to fail over to a specific recovery point. This option is useful for performing a test failover. 

Select EastWebServers-vnet-asr as the target Azure virtual network to which Azure VMs in the secondary region will be connected after the failover occurs.   

To start the failover, click OK. To track progress, click the VM to open its properties or the alert in the Notifications window. Lastly, you can click the Test Failover job in the vault name > Settings > Jobs > Site Recovery jobs. 

After the failover finishes (Start the virtual machine is successful), the replica Azure VM appears in the Azure portal > Virtual Machines. Make sure that the VM is running, sized appropriately, and connected to the appropriate network.  

 

To delete the VMs that were created during the test failover, in your vault under Protected Items > Replicated items, select your VM and then click the Content Menu (the three buttons on the right) and choose Cleanup test failover. In Notes, record and save any observations associated with the test failover. Click the box for Testing is complete and click Ok. 

If you don’t delete the failover VM, the VM will continue to run and increase your Azure consumption. 

 