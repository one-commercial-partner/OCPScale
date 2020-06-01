# Azure Backup

## Lab scenario

You have been tasked with evaluating the use of Azure Recovery Services for backup and restore of files hosted on Azure virtual machines and on-premises computers. In addition, you want to identify methods of protecting data stored in the Recovery Services vault from accidental or malicious data loss.

## Objectives

In this lab, you will:

+ Exercise 1: Provision the lab environment
+ Exercise 2: Create a Recovery Services vault
+ Exercise 3: Implement Azure virtual machine-level backup
+ Exercise 4: Implement File and Folder backup
+ Exercise 5: Perform file recovery by using Azure Recovery Services agent

## Exercise 1: Provision Virtual Machines

In this task you use the Azure CLI to create an Azure Virtual Machine running Windows Server 2019.

>**Note:** In the following steps change the `--location` valaue to a region closest to you.

1. Open an Azure CLI window by browsing to [Azure Shell](https://shell.azure.com).
2. If prompted, login using your Microsoft Azure Account.
3. When the **Welcome to Azure Cloud Shell** screen appears select **Bash** as the working CLI and then **Create Storage**.  Once storage is created click **Close**.
4. At the CLI prompt, let's create a new resource group to hold your virtual machine. Create the resource group by typing in the following command:

    ```PowerShell
    az group create --name Backup-lab --location eastus
    ```

5. Create a network security group:

    ```PowerShell
    az network nsg create --name Backup-NSG --resource-group Backup-lab --location eastus
    ```

6. Create a network security group rule for port 3389.

    ```PowerShell
    az network nsg rule create --name PermitRDP --nsg-name Backup-NSG --priority 1000 --resource-group Backup-lab --access Allow --source-address-prefixes "*" --source-port-ranges "*" --direction Inbound --destination-port-ranges 3389
    ```

7. Create a virtual network.

    ```PowerShell
    az network vnet create --name Backup-VNet --resource-group Backup-lab --address-prefixes 10.10.0.0/16 --location eastus
    ```

8. Create a subnet:

    ```PowerShell
    az network vnet subnet create --address-prefix 10.10.10.0/24 --name Backup-Subnet --resource-group Backup-lab --vnet-name Backup-VNet --network-security-group Backup-NSG
    ```

9. Create your first virtual machine:

    ```PowerShell
    az vm create --resource-group Backup-lab --name VM1 --size Standard_D2s_v3 --image Win2019Datacenter --admin-username backupadmin --admin-password Complex.Password --nsg Backup-NSG --private-ip-address 10.10.10.11 --no-wait
    ```

10. Create your second virtual machine:

    ```PowerShell
    az vm create --resource-group Backup-lab --name VM2 --size Standard_D2s_v3 --image Win2019Datacenter --admin-username backupadmin --admin-password Complex.Password --nsg Backup-NSG --private-ip-address 10.10.10.12
    ```

## Exercise 2: Create a Recovery Services vault

In this task, you will create a recovery services vault.

1. In the Azure portal, search for and select **Recovery Services vaults** and, on the **Recovery Services vaults** blade, click **+ Add**.

1. On the **Create Recovery Services vault** blade, specify the following settings:

    | Settings | Value |
    | --- | --- |
    | Subscription | the name of the Azure subscription you are using in this lab |
    | Resource group | **Backup-lab** |
    | Name | **Backup-lab-rsv1** |
    | Region | the name of a region where you deployed the two virtual machines in the previous task |

    >**Note**: Make sure that you specify the same region into which you deployed virtual machines in the previous task.

1. Click **Review + Create** and then click **Create**.

    >**Note**: Wait for the deployment to complete. The deployment should take less than 1 minute.

1. When the deployment is completed, click **Go to Resource**.

1. On the **Backup-lab-rsv1** Recovery Services vault blade, in the **Settings** section, click **Properties**.

1. On the **Backup-lab-rsv1 - Properties** blade, click the **Update** link under **Backup Configuration** label.

1. On the **Backup Configuration** blade, note that you can set the **Storage replication type** to either **Locally-redundant** or **Geo-redundant**. Leave the default setting of **Geo-redundant** in place and close the blade.

    >**Note**: This setting can be configured only if there are no existing backup items.

1. Back on the **Backup-lab-rsv1 - Properties** blade, click the **Update** link under **Security Settings** label.

1. On the **Security Settings** blade, note that **Soft Delete (For Azure Virtual Machines)** is **Enabled**.

1. Close the **Security Settings** blade and, back on the **Backup-lab-rsv1** Recovery Services vault blade, click **Overview**.

## Exercise 3: Implement Azure virtual machine-level backup

In this task, you will implement Azure virtual-machine level backup.

   >**Note**: Before you start this task, make sure that VM1 and VM2 are fully provisioned.

1. On the **Backup-lab-rsv1** Recovery Services vault blade, click **+ Backup**.

1. On the **Backup Goal** blade, specify the folowing settings:

    | Settings | Value |
    | --- | --- |
    | Where is your workload running? | **Azure** |
    | What do you want to backup? | **Virtual machine** |

1. On the **Backup Goal** blade, click **Backup**.

1. On the **Backup policy**, review the **DefaultPolicy** settings, and, in the **Choose backup policy** drop-down list, select **Create New**.

1. Define a new backup policy with the following settings (leave others with their default values):

    | Setting | Value |
    | ---- | ---- |
    | Policy name | **Backup-lab-policy** |
    | Frequency | **Daily** |
    | Time | **12:00 AM** |
    | Timezone | the name of your local time zone |
    | Retain instant recovery snapshot(s) for | **2** Days(s) |

1. Click **OK** to create the policy and then **Add**.

1. On the **Select virtual machines** blade, select **VM1** from the **Backup-lab** Resource group, click **OK**, and, back on the **Backup** blade, click **Enable backup**.

    >**Note**: Wait for the backup to be enabled. This should take about 2 minutes.

1. Navigate back to the **Backup-lab-rsv1** Recovery Services vault blade, in the **Protected items** section, click **Backup items**, and then click the **Azure virtual machines** entry.

1. On the **Backup Items (Azure Virtual Machine)** blade of **VM1**, review the values of the **Backup Pre-Check** and **Last Backup Status** entries, and click the **VM1** entry.

1. On the **VM1** Backup Item blade, click **Backup now**, accept the default value in the **Retain Backup Till** drop-down list, and click **OK**.

    >**Note**: Do not wait for the backup to complete but instead proceed to the next Exercise.

## Exercise 4: Implement File and Folder backup

In this task, you will implement file and folder backup by using Azure Recovery Services.

1. In the Azure portal, search for and select **Virtual machines**, and on the **Virtual machines** blade, click **VM2**.

1. On the **VM2** blade, click **Connect**, in the drop-down menu, click **RDP**, on the **Connect with RDP** blade, click **Download RDP File** and follow the prompts to start the Remote Desktop session.

    >**Note**: This step refers to connecting via Remote Desktop from a Windows computer. On a Mac, you can use Remote Desktop Client from the Mac App Store and on Linux computers you can use an open source RDP client software.

    >**Note**: You can ignore any warning prompts when connecting to the target virtual machines.

1. When prompted, sign in by using the **backupadmin** username and **Complex.Password** password.

1. Within the Remote Desktop session to the **VM2** Azure virtual machine, in the **Server Manager** window, click **Local Server**, click **IE Enhanced Security Configuration** and turn it **Off** for Administrators.

1. Within the Remote Desktop session to the **VM2** Azure virtual machine, start Internet Explorer, browse to the [Azure portal](https://portal.azure.com), and sign in using your credentials.

1. In the Azure portal, search for and select **Recovery Services vaults** and, on the **Recovery Services vaults**, click **Backup-lab-rsv1**.

1. On the **Backup-lab-rsv1** Recovery Services vault blade, click **+ Backup**.

1. On the **Backup Goal** blade, specify the following settings:

    | Settings | Value |
    | --- | --- |
    | Where is your workload running? | **On-premises** |
    | What do you want to backup? | **Files and folders** |

    >**Note**: Even though the virtual machine you are using in this task is running in Azure, you can leverage it to evaluate the backup capabilities applicable to any on-premises computer running Windows Server operating system.

1. On the **Backup Goal** blade, click **Prepare infrastructure**.

1. On the **Prepare infrastructure** blade, click the **Download Agent for Windows Server or Windows Client** link.

1. When prompted, click **Run** to start installation of **MARSAgentInstaller.exe** with the default settings.

    >**Note**: On the **Microsoft Update Opt-In** page of the **Microsoft Azure Recovery Services Agent Setup Wizard**, select the **I do not want to use Microsoft Update** installation option.

1. On the **Installation** page of the **Microsoft Azure Recovery Services Agent Setup Wizard**, click **Proceed to Registration**. This will start **Register Server Wizard**.

1. Switch to the Internet Explorer window displaying the Azure portal, on the **Prepare infrastructure** blade, select the checkbox **Already downloaded or using the latest Recovery Server Agent**, and click **Download**.

1. When prompted, whether to open or save the vault credentials file, click **Save**. This will save the vault credentials file to the local Downloads folder.

1. Switch back to the **Register Server Wizard** window and, on the **Vault Identification** page, click **Browse**.

1. In the **Select Vault Credentials** dialog box, browse to the **Downloads** folder, click the vault credentials file you downloaded, and click **Open**.

1. Back on the **Vault Identification** page, click **Next**.

1. On the **Encryption Setting** page of the **Register Server Wizard**, click **Generate Passphrase**.

1. On the **Encryption Setting** page of the **Register Server Wizard**, click the **Browse** button next to the **Enter a location to save the passphrase** drop-down list.

1. In the **Browse For Folder** dialog box, select the **Documents** folder and click **OK**.

1. Click **Finish**, review the **Microsoft Azure Backup** warning and click **Yes**, and wait for the registration to complete.

    >**Note**: In a production environment, you should store the passphrase file in a secure location other than the server being backed up.

1. On the **Server Registration** page of the **Register Server Wizard**, review the warning regarding the location of the passphrase file, ensure that the **Launch Microsoft Azure Recovery Services Agent** checkbox is selected and click **Close**. This will automatically open the **Microsoft Azure Backup** console.

1. In the **Microsoft Azure Backup** console, in the **Actions** pane, click **Schedule Backup**.

1. In the **Schedule Backup Wizard**, on the **Getting started** page, click **Next**.

1. On the **Select Items to Backup** page, click **Add Items**.

1. In the **Select Items** dialog box, expand **C:\\Windows\\System32\\drivers\\etc\\**, select **hosts**, and then click **OK**:

1. On the **Select Items to Backup** page, click **Next**.

1. On the **Specify Backup Schedule** page, ensure that the **Day** option is selected, in the first drop-down list box below the **At following times (Maximum allowed is three times a day)** box, select **4:30 AM**, and then click **Next**.

1. On the **Select Retention Policy** page, accept the defaults, and then click **Next**.

1. On the **Choose Initial Backup type** page, accept the defaults, and then click **Next**.

1. On the **Confirmation** page, click **Finish**. When the backup schedule is created, click **Close**.
  
1. In the **Microsoft Azure Backup** console, in the Actions pane, click **Back Up Now**.

    >**Note**: The option to run backup on demand becomes available once you create a scheduled backup.

1. In the Back Up Now Wizard, on the **Select Backup Item** page, ensure that the **Files and Folders** option is selected and click **Next**.

1. On the **Retain Backup Till** page, accept the default setting and click **Next**.

1. On the **Confirmation** page, click **Back Up**.

1. When the backup is complete, click **Close**, and then close Microsoft Azure Backup.

1. Switch to the Internet Explorer window displaying the Azure portal, navigate back to the Recovery Services vault blade and click **Backup items**.

1. On the **Backup-lab-rsv1 - Backup items** blade, click **Azure Backup Agent**.

1. On the **Backup Items (Azure Backup Agent)** blade, verify that there is an entry referencing the **C:\\** drive of **VM2**.

## Exercise 5: Perform file recovery by using Azure Recovery Services agent (optional)

In this task, you will perform file restore by using Azure Recovery Services agent.

1. Within the Remote Desktop session to **VM2**, open File Explorer, navigate to the **C:\\Windows\\System32\\drivers\\etc\\** folder and delete the **hosts** file.

1. Switch to the Microsoft Azure Backup window and click **Recover data**. This will start **Recover Data Wizard**.

1. On the **Getting Started** page of **Recover Data Wizard**, ensue that **This server (VM2)** option is selected and click **Next**.

1. On the **Select Recovery Mode** page, ensure that **Individual files and folders** option is selected, and click **Next**.

1. On the **Select Volume and Date** page, in the **Select the volume** drop down list, select **C:\\**, accept the default selection of the available backup, and click **Mount**.

    >**Note**: Wait for the mount operation to complete. This might take about 2 minutes.

1. On the **Browse And Recover Files** page, note the drive letter of the recovery volume and review the tip regarding the use of robocopy.

1. Click **Start**, expand the **Windows System** folder, and click **Command Prompt**.

1. From the Command Prompt, run the following to copy the restore the **hosts** file to the original location (replace `[recovery_volume]` with the drive letter of the recovery volume you identified earlier):

   ```
   robocopy [recovery_volume]:\Windows\System32\drivers\etc C:\Windows\system32\drivers\etc hosts /r:1 /w:1
   ```

1. Switch back to the **Recover Data Wizard** and, on the **Browse and Recover Files**, click **Unmount** and, when prompted to confirm, click **Yes**.

1. Terminate the Remote Desktop session.

## Review

In this lab, you have:

+ Exercise 1: Provision the lab environment
+ Exercise 2: Create a Recovery Services vault
+ Exercise 3: Implement Azure virtual machine-level backup
+ Exercise 4: Implement File and Folder backup
+ Exercise 5: Perform file recovery by using Azure Recovery Services agent

## [Return to All Labs](../index.md)