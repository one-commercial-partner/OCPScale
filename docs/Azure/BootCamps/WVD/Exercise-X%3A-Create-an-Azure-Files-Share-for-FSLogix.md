Exercise X: Create an Azure Files Share for FSLogix
-----------------------------------------------------
---
In this exercise you will be creating an Azure Files share and enabling Active Directory authentication for use in your WVD tenant. Azure Files is a platform service (PaaS) and is the recommended for hosting FSLogix containers for WVD users. At the end of this exercise you will have the following components:

- A new storage account in your Azure subscription.
- A new Azure file share for your FSLogix profile containers.
- AD authentication enabled for your Azure storage account.
- Permissions applied for user access to the file share.

[Source Article](https://docs.microsoft.com/en-us/azure/storage/files/storage-files-quick-create-use-windows)

---

## Task 1: Create a storage account

Before you can work with an Azure file share, you have to create an Azure storage account. A general-purpose v2 storage account provides access to all of the Azure Storage services: blobs, files, queues, and tables. A storage account can contain an unlimited number of shares. A share can store an unlimited number of files, up to the capacity limits of the storage account.

To create a general-purpose v2 storage account in the Azure portal, follow these steps:

1. Sign in to the [Azure Portal](https://portal.azure.com/)

2. On the Azure portal menu, select **All services**. In the list of resources, type *Storage Accounts*. As you begin typing, the list filters based on your input. Select **Storage Accounts**.

3. On the Storage Accounts window that appears, click **+ Add**.

4. Fill in the required parameters for the storage account. Refer to the following example for more information on the available parameters.

   ![SD-Ex9000.png](/.attachments/SD-Ex9000-8eccf4dd-29b8-44c3-89c4-396fa11e180a.png)

5. Select **Review + Create** to review your storage account settings and create the account.

   ![SD-Ex9001.png](/.attachments/SD-Ex9001-9f9cde2b-5498-4ff2-8f14-74e2470b1334.png =50%x)

6. Click **Create**.

---

## Task 2: Create an Azure file share

1. When the Azure storage account deployment is complete, click **Go to resource**.

2. On the Overview page for the new storage group, select **File shares**.

   ![SD-Ex9002.png](/.attachments/SD-Ex9002-1991aee8-a06f-4b91-9b85-c35f8ec0d639.png =60%x)

3. On the File shares blade, click **+ File Share**.

4. Name the new file share **fslogixprofiles**, enter **20** for the Quota and click **Create**.

   > **Note:** The file share quota supports a maximum of 5,120 GiB and can be managed on the File shares blade.

   ![SD-Ex9003.png](/.attachments/SD-Ex9003-b9beffb1-7b2a-483c-9fc0-0f1c7b0d266a.png =30%x)

---

## Task 3: Enable AD authentication for your storage account

To enable AD authentication over SMB for Azure file shares, you need to first register your storage account with AD and then set the required domain properties on the storage account. When the feature is enabled on the storage account, it applies to all new and existing file shares in the account. Use `join-AzStorageAccountForAuth` to enable the feature. You can find the detailed description of the end-to-end workflow in the section below. 

> [!IMPORTANT]
> The `Join-AzStorageAccountForAuth` cmdlet will make modifications to your AD environment. Read the following explanation to better understand what it is doing to ensure you have the proper permissions to execute the command and that the applied changes align with the compliance and security policies. 

The `Join-AzStorageAccountForAuth` cmdlet will perform the equivalent of an offline domain join on behalf of the indicated storage account. It will create an account in your AD domain, either a computer account or a service logon account. The created AD account represents the storage account in the AD domain. If the AD account is created under an AD Organizational Unit (OU) that enforces password expiration, you must update the password before the maximum password age. Failing to update AD account password will result in authentication failures when accessing Azure file shares. To learn how to update the password, see [Update AD account password](#update-ad-account-password).

You can use the following script to perform the registration and enable the feature.

**1. Check prerequisites**
- Download and unzip the [AzFilesHybrid module](https://github.com/Azure-Samples/azure-files-samples/releases).
- Install and execute the module on a device that is domain joined to AD with AD credentials that have permissions to create a service logon account or a computer account in the target AD.
-  Run the script using an AD credential that is synced to your Azure AD. The AD credential must have either the storage account owner or the contributor RBAC role permissions.
- Make sure your storage account is in a [supported region](#regional-availability).

### 2. Domain join your storage account
Remember to replace the placeholder values with your own in the parameters below before executing it in PowerShell.

```PowerShell
#Change the execution policy to unblock importing AzFilesHybrid.psm1 module
Set-ExecutionPolicy -ExecutionPolicy Unrestricted -Scope CurrentUser

# Navigate to where AzFilesHybrid is unzipped and stored and run to copy the files into your path
.\CopyToPSPath.ps1 

#Import AzFilesHybrid module
Import-Module -Name AzFilesHybrid

#Login with an Azure AD credential that has either storage account owner or contributer RBAC assignment
Connect-AzAccount

#Select the target subscription for the current session
Select-AzSubscription -SubscriptionId "<your-subscription-id-here>"

# Register the target storage account with your active directory environment under the target OU (for example: specify the OU with Name as "UserAccounts" or DistinguishedName as "OU=UserAccounts,DC=CONTOSO,DC=COM"). 
# You can use to this PowerShell cmdlet: Get-ADOrganizationalUnit to find the Name and DistinguishedName of your target OU. If you are using the OU Name, specify it with -OrganizationalUnitName as shown below. If you are using the OU DistinguishedName, you can set it with -OrganizationalUnitDistinguishedName.
# You can choose to create the identity that represents the storage account as either a Service Logon Account or Computer Account, depends on the AD permission you have and preference. 
Join-AzStorageAccountForAuth `
        -ResourceGroupName "<resource-group-name-here>" `
        -Name "<storage-account-name-here>" `
        -DomainAccountType "ComputerAccount" `
        -OrganizationalUnitName "<ou-name-here>"
```

### 3. Confirm that the feature is enabled

You can check to confirm whether the feature is enabled on your storage account, you can use the following script:

```PowerShell
# Get the target storage account
$storageaccount = Get-AzStorageAccount `
        -ResourceGroupName "<your-resource-group-name-here>" `
        -Name "<your-storage-account-name-here>"

# List the directory service of the selected service account
$storageAccount.AzureFilesIdentityBasedAuth.DirectoryServiceOptions

# List the directory domain information if the storage account has enabled AD authentication for file shares
$storageAccount.AzureFilesIdentityBasedAuth.ActiveDirectoryProperties
```

You've now successfully enabled the feature on your storage account. Even though the feature is enabled, you still need to complete additional actions to be able to use the feature properly.

[!INCLUDE [storage-files-aad-permissions-and-mounting](../../../includes/storage-files-aad-permissions-and-mounting.md)]

You have now successfully enabled AD authentication over SMB and assigned a custom role that provides access to an Azure file share with an AD identity. To grant additional users access to your file share, follow the instructions in the [Assign access permissions](#assign-access-permissions-to-an-identity) to use an identity and [Configure NTFS permissions over SMB](#configure-ntfs-permissions-over-smb) sections.

## Update AD account password

If you registered the AD identity/account representing your storage account under an OU that enforces password expiration time, you must rotate the password before the maximum password age. Failing to update the password of the AD account will result in authentication failures to access Azure file shares.  

To trigger password rotation, you can run the `Update-AzStorageAccountADObjectPassword` command from the AzFilesHybrid module. The cmdlet performs actions similar to storage account key rotation. It gets the second Kerberos key of the storage account and uses it to update the password of the registered account in AD. Then it regenerates the target Kerberos key of the storage account and updates the password of the registered account in AD. You must run this cmdlet in an AD domain joined environment.

```PowerShell
#Update the password of the AD account registered for the storage account
Update-AzStorageAccountADObjectPassword `
        -RotateToKerbKey kerb2 `
        -ResourceGroupName "<your-resource-group-name-here>" `
        -StorageAccountName "<your-storage-account-name-here>"
```