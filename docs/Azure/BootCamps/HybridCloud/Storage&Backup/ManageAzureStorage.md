
# Manage Azure Storage

## Lab scenario

You need to evaluate the use of Azure storage for storing files residing currently in on-premises data stores. While majority of these files are not accessed frequently, there are some exceptions. You would like to minimize cost of storage by placing less frequently accessed files in lower-priced storage tiers. You also plan to explore different protection mechanisms that Azure Storage offers, including network access, authentication, authorization, and replication. Finally, you want to determine to what extent Azure Files service might be suitable for hosting your on-premises file shares.

## Objectives

In this lab, you will:

+ Task 1: Provision the lab environment
+ Task 2: Create and configure Azure Storage accounts
+ Task 3: Manage blob storage
+ Task 4: Manage authentication and authorization for Azure Storage
+ Task 5: Create and configure an Azure Files shares

## Task 1: Provision the lab environment

In this task you use the Azure CLI to create an Azure Virtual Machine running Windows Server 2019.

>**Note:** In the following steps change the `--location` value to a region closest to you.

1. Open an Azure CLI window by browsing to [Azure Shell](https://shell.azure.com).
2. If prompted, login using your Microsoft Azure Account.
3. When the **Welcome to Azure Cloud Shell** screen appears select **Bash** as the working CLI and then **Create Storage**.  Once storage is created click **Close**.
4. At the CLI prompt, let's create a new resource group to hold your virtual machine. Create the resource group by typing in the following command:

    ```PowerShell
    az group create --name Storage-Lab --location eastus
    ```

5. Create a network security group:

    ```PowerShell
    az network nsg create --name Storage-NSG --resource-group Storage-Lab --location eastus
    ```

6. Create a network security group rule for port 3389.

    ```PowerShell
    az network nsg rule create --name PermitRDP --nsg-name Storage-NSG --priority 1000 --resource-group Storage-Lab --access Allow --source-address-prefixes "*" --source-port-ranges "*" --direction Inbound --destination-port-ranges 3389
    ```

7. Create a virtual network.

    ```PowerShell
    az network vnet create --name Storage-VNet --resource-group Storage-Lab --address-prefixes 10.10.0.0/16 --location eastus
    ```

8. Create a subnet:

    ```PowerShell
    az network vnet subnet create --address-prefix 10.10.10.0/24 --name Storage-Subnet --resource-group Storage-Lab --vnet-name Storage-VNet --network-security-group Storage-NSG
    ```

9. Create your virtual machine:.

    ```PowerShell
    az vm create --resource-group Storage-Lab --name VM1 --size Standard_D2s_v3 --image Win2019Datacenter --admin-username storadmin --admin-password Complex.Password --nsg Storage-NSG --private-ip-address 10.10.10.11
    ```

    > It will take several minutes to provision the virtual machine.  We suggest you write down the credentials to VM1 and move onto Task 2.

## Task 2: Create and configure Azure Storage accounts

In this task, you will create and configure an Azure Storage account.

1. In the Azure portal, search for and select **Storage accounts**, and then click **+ Add**.
1. On the **Basics** tab of the **Create storage account** blade, specify the following settings (leave others with their default values):

    | Setting | Value |
    | --- | --- |
    | Subscription | the name of the Azure subscription you are using in this lab |
    | Resource group |  **Storage-Lab** |
    | Storage account name | any globally unique name between 3 and 24 in length consisting of letters and digits |
    | Location | the name of an Azure region where you can create an Azure Storage account  |
    | Performance | **Standard** |
    | Account kind | **Storage (general purpose v1)** |
    | Replication | **Read-access geo-redundant storage (RA-GRS)** |

1. Click **Next: Networking >**, on the **Networking** tab of the **Create storage account** blade, review the available options, accept the default option **Public endpoint (all networks)** and click **Next: Advanced >**.

1. On the **Advanced** tab of the **Create storage account** blade, review the available options, accept the defaults, click **Review + Create**, wait for the validation process to complete and click **Create**.

    >**Note**: Wait for the Storage account to be created. This should take about 2 minutes.

1. On the deployment blade, click **Go to resource** to display the Azure Storage account blade.

1. On the Azure Storage account blade, in the **Settings** section, click **Configuration**.

1. Click **Upgrade** to change the Storage account kind from **Storage (general purpose v1)** to **StorageV2 (general purpose v2)**.
1. On the **Upgrade storage account** blade, review the warning stating that the upgrade is permanent and will result in billing charges, in the **Confirm upgrade** text box, type the name of the storage account, and click **Upgrade**.

    > **Note**: You have the option to set the account kind to **StorageV2 (general purpose v2)** at the provisioning time. The previous two steps were meant to illustrate that you also have the option to upgrade existing general purpose v1 accounts.

    > **Note**: **StorageV2 (general purpose v2)** offers a number of features, such as, for example, access tiering, not available in with general purpose v1 accounts.

    > **Note**: Review the other configuration options, including **Access tier (default)**, currently set to **Hot**, which you can change, the **Performance**, currently set to **Standard**, which can be set only during account provisioning, and the **Identity-based Directory Service for Azure File Authentication**, which requires Azure Active Directory Domain Services.

1. On the Storage account blade, in the **Settings** section, click **Geo-replication** and note the secondary location. Click the **View all** link under the **Storage endpoints** label and review the **Storage account endpoints** blade.  

    > **Note**: As expected, the **Storage account endpoints** blade contains both primary and secondary endpoints.

1. Switch to the Configuration blade of the Storage account and, in the **Replication** drop-down list, select **Geo-redundant storage (GRS)** and save the change.

1. Switch back to the **Geo-replication** blade and note that the secondary location is still specified. Click the **View all** link under the **Storage endpoints** label and review the **Storage account endpoints** blade.  

    > **Note**: As expected, the **Storage account endpoints** blade contains only primary endpoints.

1. Display again the **Configuration** blade of the Storage account, in the **Replication** drop-down list select **Locally redundant storage (LRS)** and save the change.

1. Switch back to the **Geo-replication** blade and note that, at this point, the Storage account has only the primary location.

1. Display again the **Configuration** blade of the Storage account and set **Access tier (default)** to **Cool**.

    > **Note**: The cool access tier is optimal for data which is not accessed frequently.

## Task 3: Manage blob storage

In this task, you will create a blob container and upload a blob into it.

1. On the Storage account blade, in the **Blob service** section, click **Containers**.

1. Click **+ Container** and create a container with the following settings:

    | Setting | Value |
    | --- | --- |
    | Name | **storage-lab-container**  |
    | Public access level | **Private (no anonymous access)** |

1. In the list of containers, click **storage-lab-container** and then click **Upload**.

1. Browse to your local  computer and click **Open**.

1. On the **Upload blob** blade, expand the **Advanced** section and specify the following settings (leave others with their default values):

    | Setting | Value |
    | --- | --- |
    | Authentication type | **Account key**  |
    | Blob type | **Block blob** |
    | Block size | **4 MB** |
    | Access tier | **Hot** |
    | Upload to folder | **localfiles** |

    > **Note**: Access tier can be set for individual blobs.

1. Click **Upload**.

    > **Note**: Note that the upload automatically created a subfolder named **localfiles**.

1. Back on the **storage-lab-container** blade, click **localfiles** and then click the file you uploaded.

1. On the **localfiles** blade, review the available options.

    > **Note**: You have the option to download the blob, change its access tier (it is currently set to **Hot**), acquire a lease, which would change its lease status to **Locked** (it is currently set to **Unlocked**) and protect the blob from being modified or deleted, as well as assign custom metadata (by specifying an arbitrary key and value pairs). You also have the ability to **Edit** the file directly within the Azure portal interface, without downloading it first. You can also create snapshots, as well as generate a SAS token (you will explore this option in the next task).

## Task 4: Manage authentication and authorization for Azure Storage

In this task, you will configure authentication and authorization for Azure Storage.

1. On the **Overview** tab, click **Copy to clipboard** button next to the **URL** entry.

1. Open another browser window by using InPrivate mode and navigate to the URL you copied in the previous step.

1. You should be presented with an XML-formatted message stating **ResourceNotFound**.

    > **Note**: This is expected, since the container you created has the public access level set to **Private (no anonymous access)**.

1. Close the InPrivate mode browser window, return to the browser window showing the **localfiles** blade of the Azure Storage container.

1. On the file you uploaded, click the context menu (the three dots ...) and select **Generate SAS**. Specify the following settings (leave others with their default values):

    | Setting | Value |
    | --- | --- |
    | Permissions | **Read** |
    | Start date | yesterday's date |
    | Start time | current time |
    | Expiry date | tomorrow's date |
    | Expiry time | current time |
    | Allowed IP addresses | leave blank |
    | Allowed protocols | **HTTP** |
    | Signing key | **Key 1** |

1. Click **Generate SAS token and URL**.

1. Click **Copy to clipboard** button next to the **Blob SAS URL**.

1. Open another browser window by using InPrivate mode and navigate to the URL you copied in the previous step.

    > **Note**: Save the blob SAS URL. You will need it later in this lab.

## Task 5: Create and configure an Azure Files shares

In this task, you will create and configure Azure Files shares.

   > **Note**: Before you start this task, verify that VM1 you provisioned in the first task of this lab is running.

1. In the Azure portal, navigate back to the blade of the storage account you created in the first task of this lab and, in the **File service** section, click **File shares**.

1. Click **+ File share** and create a file share with the following settings:

    | Setting | Value |
    | --- | --- |
    | Name | **storage-lab-share** |
    | Quota | **1024** |

1. Click the newly created file share and click **Connect**.

1. On the **Connect** blade, ensure that the **Windows** tab is selected, and click **Copy to clipboard**.

1. In the Azure portal, search for and select **Virtual machines**, and, in the list of virtual machines, click **VM1**.

1. On the **VM1** blade, in the **Operations** section, click **Run command**.

1. On the **VM1 - Run command** blade, click **RunPowerShellScript**.

1. On the **Run Command Script** blade, paste the script you copied earlier in this task into the **PowerShell Script** pane and click **Run**.

1. Verify that the script completed successfully.  You should have the letter Z: mapped to the Azure Files share. (How cool is that!)

1. Replace the content of the **PowerShell Script** pane with the following script and click **Run**:

   ```pwsh
   New-Item -Type Directory -Path 'Z:\storage-lab-folder'

   New-Item -Type File -Path 'Z:\storage-lab-folder\storage-lab-file.txt'
   ```

1. Verify that the script completed successfully.

1. Navigate back to the **storage-lab-share** file share blade, click **Refresh**, and verify that **storage-lab-folder** appears in the list of folders.

1. Click **storage-lab-folder** and verify that **storage-lab-file.txt** appears in the list of files.

## Review

In this lab, you completed the following:

+ Task 1: Provision the lab environment
+ Task 2: Create and configure Azure Storage accounts
+ Task 3: Manage blob storage
+ Task 4: Manage authentication and authorization for Azure Storage
+ Task 5: Create and configure an Azure Files shares

**If you are moving to the Backup lab, you'll need VM1 for that lab, otherwise you can delete all the resources from your subscription.**

## [Continue to Backup Lab](AzureBackup.md)

## [Return to All Labs](../index.md)
