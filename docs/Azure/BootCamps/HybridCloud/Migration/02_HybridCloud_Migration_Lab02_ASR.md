# Azure Site Recovery for Migration  

In this lab you will create a VM in Azure to simulate a source VM running in either VMware or Hyper-V on the ground.  We will then replicate (aka migrate) the VM to Azure.

Please note that using this approach represents `the fastest way` to migrate a VM to Azure and should not be seen as the usual, customary amount of time it takes to perform a migration to Azure.

## Exercise 1 - Create an IIS VM with PowerShell

In this task you use the Azure CLI to create an Azure Virtual Machine running Windows Server 2016, and install IIS.

### Task 1 - Provision a Virtual Machine

1. Open an Azure CLI window by browsing to [Azure Shell](https://shell.azure.com).
2. Login using your Microsoft Account.
3. If prompted, select the default AAD directory.
4. If a Welcome to Azure Cloud Shell prompt appears after logon, select Bash as the working CLI. If it does not appear, you can select Bash from the dropdown in the upper-left corner once a CLI prompt is presented to you. Note that you may need to provision a new CLI storage account to save your settings.
5. At the CLI prompt, let's create a new resource group to hold your IIS VM. Create the resource group by typing in the following command:

    `az group create --name Migration --location eastus`

6. Create the VM by typing in the following command

    >Note: Change your first name and if needed, location to an appropriate Region.

    `az vm create --resource-group Migration --name IIS --location eastus --image win2016datacenter
 --admin-username *yourfirstname*
 --admin-password Complex.Password
 --size "Standard_DS2_v2"`
7. Once the VM is created, let's open port 80 so we can access the VM's website from the internet. Run the follwing command:

    `az vm open-port --port 80 --resource-group Migration --name IIS`

### Task 2 - Install IIS

Now let's install IIS using PowerShell.

1. RDP into the IIS VM, open **Windows PowerShell** and enter the following:

    `Install-WindowsFeature -name "Web-Server" -IncludeManagementTools`

## Exercise 2 - Create target resources

We could have ASR automatically create the target resources (i.e. Resource groups, virtual networks, and subnets) but in a more realistic scenario you'd want to pre-create these resources and place your migrated VMs in specific networks.

### Task 1 - Create Target Resource Groups

1. Back on your desktop and in the Azure Portal, click on **Microsoft Azure > Resource groups > +Add**.
2. Enter the following: 
    * Resource group: **Target-RG**
    * Region: **(US) West US**
    * Click **Review + Create**
    * Click **Create**

### Task 2 - Create Target Virtual Networks

1. Click on **Microsoft Azure > Networking >** and then type in the search box **Virtual networks**.
2. Enter the following:
    * Name: **Target-vNet**
    * Resource Group: **Target-RG**
    * Location: **(US) West US**
    * Click **Next: IP Addresses >**
3. Click on the trash can to remove the existing IPv4 address space.
4. Under **IPv4 address space** enter **10.100.0.0/16**.
5. Click on **+ Add subnet** and enter **Target-subnet** as the Subnet name and **10.100.10.0/24** as the subnet address range.
6. Click **Add**, then **Review + Create**, then **Create**.

## Exercise 3 - Create a Recovery Services vault

This task is the normal starting point for a typical lift and shift migration as you would normally have a robust source environment to migrate.

1. Click **+Create a resource > IT & Management Tools > Backup and Site Recovery** and create a new Resource group named **ASRMigration** and then enter **MyVault** as the Vault Name.  
2. Choose **Central US** as the location.
3. Click **Review + Create** and then **Create**.

## Exercise 4 - Select a replication goal

1. Once the vault deployment has succeeded, click on **Go to Resource** or in the search bar type in **MyVault** and select it.
2. In the **Getting Started** Menu, click **Site Recovery** > **Prepare Infrastructure**.
3. In **Protection goal**, set the following:
    * Where are your machines located?: **Azure**
    * Where do you want to replicate your machines to? **To Azure**
    * Click **OK**, and then **OK** again on the **Prepare Infrastructure** tab.

## Exercise 5 - Enable replication

1. In the Azure portal, click **Microsoft Azure > Virtual machines**, and select the **IIS**.
2. Scroll down to **Operations** and select click **Disaster recovery**.
3. On the **Basics** tab select **West US** as the target region to which you'll replicate.
4. Under the **Advanced settings** tab, set the following Target resources:  
    * VM resource group: **Target-RG**
    * Virtual network: **Target-vNet**
    * Availability: **Availability set** *This will create the migrated VM in an availability set*
    * Click **Next: Review + Start Replication**.
5. Review the settings and click **Start replication**. This starts a job to enable replication (aka migration) for the VM.
6. You may notice that Vaildating and Creating target resources takes a few moments to process.  The fabric is ensuring that resources in your target region can be created and there’s no conflicts nor capacity issues.

## Exercise 6 - Track Replication

1. Once Azure has built the core components replication will begin.  On the alert button (the bell) click on **Enabling replication for 1 vm(s)**.
2. Notice the steps as they occur in real time.  The longest step in the process is going to be **Enable replication**.  Select that item and observe the series of steps taking place. IR, or Initial Replication, is the time it takes the VM to be copied from source to target.  Notice the Status of IR.  
3. Since it may take 30 minutes to replicate the VM, now may be an appropriate time to take a break or come back to the lab at a later time.
4. You can check percentage complete of replication by **Virtual Machines > IIS > Operations > Disaster Recovery**.  You may notice status sits at 0% synchronized for some time and then report upwards of 87% complete on next refresh.

## Exercise 7 - Run a Test Failover

A test failover executes a failover but does not make the secondary or migrated VM active.  A drill validates your migration strategy without data loss or downtime and doesn't affect your production environment.

1. In the Azure Portal navigate to **Virtual machines**, then **IIS**, then **Disaster Recovery**.  Click the **Test Failover** icon.
2. In Test Failover, select **Latest (lowest RPO)** as the recovery point to use for the failover.  Note the following:
    * **Latest (lowest RPO):** Fails the VM over with the current state of the VM but requires some processing time.
    * **Latest processed (low RTO):** Fails the VM over to the latest recovery point that was processed by the Site Recovery service. The time stamp is shown. With this option, no time is spent processing data, so it provides a low RTO (Recovery Time Objective)
    * **Latest app-consistent:** This option fails over all VMs to the latest app-consistent recovery point. The time stamp is shown.
    * **Custom:** Use this option to fail over to a specific recovery point. This option is useful for performing a test failover.
3. Select **Target-vNet** as the target Azure virtual network to which Azure VMs in the secondary region will be connected after the failover occurs.  
4. To start the failover, click **OK**. Track progress by selecting the alert in the Notifications window.
5. After the failover finishes (Start the virtual machine task is successful), the replica Azure VM appears in the Azure portal under Virtual Machines. Make sure that the VM is running, sized appropriately, and connected to the appropriate network. Note that the VM *does not* have a Public IP address.
6. To delete the VMs that were created during the test failover, select **IIS** from **Virtual Machines**, select **Disaster recovery** under  **Operations**, and then choose **Cleanup test failover**.
7. In Notes, record and save any observations associated with the test failover. Click the box for **Testing is complete** and click **Ok**.
If you don’t delete the failover VM, the VM will continue to run and increase your Azure consumption.

## Exercise 8 - Switch over to the migrated VM

 Once you have validated the migrated VM by performing a test failover, your next step would be to switch over to the migrated VM.  In this lab you will complete the migration.

 1. Note the public IP address of the IIS virtual machine.  Open a browser and surf to the public IP address.
 2. Once you have cleaned up your Test Failover, click on **Failover**.
 3. Under **Recovery Point** enure that **(low RTO)** or **Latest processed (low RTO)** is selected and click **OK**. Note the checkbox to shut down the source VM before failover (migration).
 Under alerts click the link for **Starting failover** and monitor the failover (migration).
 4. As failover starts click on **Virtual Machines** in the Azure Portal and continually hit refresh.  Eventually you will have two IIS VMs; one **Stopped (deallocated)** in the East US and then another VM in the West US that's running.

## Exercise 9 - Make your migrated VM accessible

 When you migrate a VM a public IP address is not added by default and the Virtual Network does not have any Network Security Group rules.  We need to add all of these.

 For real production migrations Microsoft recommends that you add these steps to what is called a recovery plan so that they are automatically built and added as the VM is migrated.

### Create a Public IP address

1. In the search box type in **Public IP Address**, select **Public IP addresses** and hit enter.
2. Click on **+Add** and enter the following:
    * Name: **IIS-PublicIP**
    * Resource group: **Target-RG**
    * Location: **West US**
3. Once the resource is provisioned, it needs to be associated to a network interface.  Complete the following steps:
    * Click on **Associate**
    * Resource type: **Network interface**
    * Network interface: **IISVNic**
    * Click on **OK**
4. In the Azure portal, click **Microsoft Azure > Virtual machines**, and select the **IIS** (the VM that is running).
5. Note the public IP address of the IIS virtual machine.  Open a browser and surf to the public IP address.
