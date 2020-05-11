# Hybrid Identity Hands-On Lab

## Lab scenario

In this lab you will start by building and configuring an Azure VM to simulate an on-premises Active Directory domain controller.  You will then provision a VM in Azure to run Azure Active Directory Connect.  Afterwards, you will provision an Azure Active Directory tenant, and finally, using Azure AD Connect, synchronize accounts from on-premises Active Directory to Azure Active Directory.

## Before you Begin

In order to be successful with this lab you will need a clean Azure subscription.  By clean we mean no resources in use or in production and the identity you have associated with this Azure subscription is not associated with any other active Azure subscription or Azure Active Directory.

>DO NOT CONTINUE WITH THIS LAB IF YOU MAY IMPACT PRODUCTIONS RESOURCES AND SERVICES.

You will consume approximately $10 in Azure usage building this lab.  You will also need four (4) Total Regional vCPUs available in your subscription.

## Exercise 1 - Setup an IaaS Virtual Machine via Azure CLI

In this task you use the Azure CLI to create an Azure Virtual Machine running Windows Server 2019.

1. Open an Azure CLI window by browsing to [Azure Shell](https://shell.azure.com).
2. If prompted, login using your Microsoft Azure Account.
3. When the **Welcome to Azure Cloud Shell** screen appears select **Bash** as the working CLI and then **Create Storage**.  Once storage is created click **Close**.
4. At the CLI prompt, let's create a new resource group to hold your Domain Controller. Create the resource group by typing in the following command:

    ```PowerShell
    az group create --name Identity-Infrastructure --location eastus
    ```

5. Create a network security group:

    ```PowerShell
    az network nsg create --name AD-NSG --resource-group Identity-Infrastructure --location eastus
    ```

6. Create a network security group rule for port 3389.

    ```PowerShell
    az network nsg rule create --name PermitRDP --nsg-name AD-NSG --priority 1000 --resource-group Identity-Infrastructure --access Allow --source-address-prefixes "*" --source-port-ranges "*" --direction Inbound --destination-port-ranges 3389
    ```

7. Create a virtual network.

    ```PowerShell
    az network vnet create --name AD-VNet --resource-group Identity-Infrastructure --address-prefixes 10.10.0.0/16 --location eastus
    ```

8. Create a subnet:

    ```PowerShell
    az network vnet subnet create --address-prefix 10.10.10.0/24 --name AD-Subnet --resource-group Identity-Infrastructure --vnet-name AD-VNet --network-security-group AD-NSG
    ```

9. Create your virtual machine:.

    ```PowerShell
    az vm create --resource-group Identity-Infrastructure --name DC01 --size Standard_B2s --image Win2019Datacenter --admin-username ADadmin --admin-password Complex.Password --nsg AD-NSG --private-ip-address 10.10.10.11
    ```

    > It will take several minutes to provision the virtual machine.  We suggest you write down the credentials to DC01.

10. Close Azure Shell.

## Exercise 2 - Install and Configure Active Directory

In this task you use PowerShell (or PowerShell ISE) within Windows Server 2019 to install Active Directory.

1. Once DC01 is running connect to the DC01 virtual machine and logon with your local account (`ADadmin`) by selecting **Microsoft Azure / Resource Groups / Identity-Infrastructure / DC01 / Connect / RDP**.  
2. Make sure that you choose the **public IP address**, not the *Private IP address*, and then click on **Download RDP File**.
3. Logon with your local credentials that you wrote down earlier.  You may have to choose **More Choices** then **Use a different account** to enter your new set of credentials. The username is `ADadmin` and the password is `Complex.Password`.  Click the checkbox for **Don't ask me again for connections to this computer** and then **Yes** when prompted regarding the certificate error.
4. Once your desktop is provisioned, click **No** on the Network Discovery blade.
5. Hit the **Windows Start** button and then open **Windows PowerShell**. Enter the following to install the Active Directory Domain Service module:

    ```PowerShell
    Install-windowsfeature AD-Domain-Services -IncludeAllSubFeature -IncludeManagementTools
    ```

    > This may takes a few minutes to run.

6. Import the deployment modules by entering the following:

    ```PowerShell
    Import-Module ADDSDeployment
    ```

    >PowerShell will quickly return as this command takes milliseconds to execute.
7. Promote your server to a domain controller by entering the following command.  Don't forget to set the **domain names properly** minding the quotes.

    ```PowerShell
    Install-ADDSForest -CreateDnsDelegation:$false -DatabasePath "C:\Windows\NTDS” -DomainMode “Win2012R2” -DomainName “<yourADdomain.TLD>" -DomainNetbiosName “<yourADdomain>" -ForestMode “Win2012R2” -InstallDns:$true -LogPath “C:\Windows\NTDS” -SysvolPath "C:\Windows\SYSVOL” -Force:$true
    ```

    > Write down your FQDN doman name and NetBIOS on your scratch pad for future reference.

8. Once you hit enter you will be asked for the  SafeModeAdministratorPassword – this is for the Directory Services Restore Mode (DSRM). Enter `Complex.Password` and then retype to confirm.

    > You will receive warnings about security settings, network adapters, and DNS Servers.  These warnings can be ignored.

9. Once Active Directory is installed your virtual machine will restart.

## Exercise 3 - Configure DNS

The virtual network that contains the domain controller is pointing to Azure DNS, not the DNS of the domin controller, for name resolution.  Any new VMs will not be able to find the DNS service on the domain controller and be able to join the domain.  Change the DNS to point to the domain controller.

1. In the Azure portal click **Home** -> **Resource groups** -> **Identity-Infrastructure**.
2. Click on **DC01** and copy the Private IP address (e.g. 10.10.10.11).
3. Within the **Identity-Infrastructure** Resource Group click on  **AD-Vnet**.
4. Under **Settings** click **DNS Servers**.
5. Change the DNS servers to **Custom** and paste the IP address of DC01 under `Add DNS server`.
6. Click **Save**.

## Exercise 4  - Connect to the Domain Controller and create a user account

### Create Domain Accounts

1. RDP into DC01 and logon with the following domain account:
    * Username: `adadmin@<yourdomain.tld>`
    * Password: `Complex.Password`

    > Choose **More Choices** then **Use a different account** to enter your new set of credentials.
2. In **Server Manager / Tools** open **Active Directory Users and Computers**. In the domain tree expand your domain and then select **Users**, then right-click, **New**  then **User** and create a user object with the following information:
    * First Name: **Bob**
    * Last Name: **Jones**
    * Full Name: **Bob Jones**
    * User Logon Name: **Bob.Jones**
3. Click **Next** and set the password to `Complex.Password`. Uncheck **User must change password at next logon**, and set the **Password never expires** checkbox.
4. Click **Next** then **Finish**.
5. In the domain tree select **Users**, then right-click, **New**  then **User** and create a user object with the following information:
    * First Name: **Julia**
    * Last Name: **Williams**
    * Full Name: **Julia Williams**
    * User Logon Name: **Julia.Williams**
6. Click **Next** and set the password to `Complex.Password`. Uncheck **User must change password at next logon**, and set the **Password never expires** checkbox.
7. Click **Next** then **Finish**.

## Exercise 5 - Create a virtual machine to host AD Connect

Following [Microsoft recommended practices](https://docs.microsoft.com/en-us/azure/active-directory/hybrid/how-to-connect-install-prerequisites#azure-ad-connect-server) we are creating a virtual machine to be used to host Azure AD Connect.

1. From your desktop return to and open an Azure CLI window by browsing to [Azure Shell](https://shell.azure.com).
2. You may have to hit **Reconnect**.
3. Create your virtual machine:

    ```PowerShell
    az vm create --resource-group Identity-Infrastructure --name ADConnect --size Standard_B2s --image Win2019Datacenter --admin-username ADConnectAdmin --admin-password Complex.Password --nsg AD-NSG --private-ip-address 10.10.10.15
    ```

## Exercise 6 - Install Azure Active Directory

1. In the Azure Portal, click **Microsoft Azure** and then **+Create a resource**.  Select **Identity** and then **Azure Active Directory**.
2. Enter the following on the **Create directory** tab:
    * Organization name: **Identity Lab**
    * Initial domain name: `<yourinitials>`IdentityLab *(e.g. abdIdentityLab)*
    * Hit **Tab**.

        >Ensure validation passes as your namespace needs to be unique within the onmicrosoft.com namespace.  We often see students choosing a domain name that already exists.

3. Click **Create**.  It will take several minutes for the directory to be created.

## Exercise 7 - Create a Sync Account

We are going to create an account that AD Connect will use to perform the synchronization process between the on-prem domain controller and Azure Active Directory.

1. In Azure Active Directory click on **+ New User** and enter the following:
    * User name: **AzureADSync**
    * Name: **AzureADSync**
    * Under Password:
        * Select **Let me create the password**
        * Initial password: `Temporary.Password`
    * Under **Groups and roles** click on **User**
        * Scroll down to find and select `Global administrator`.
        * Click **Select**
    * Usage location: **United States**
2. Click **Create**.

### Reset the Sync Account password

1. Open an InPrivate or Incognito browser.
2. Surf to portal.azure.com.
3. Logon as `AzureADSync@<yourAzureADdomainname>.onmicrosoft.com` with a password of `Temporary.Password`.
4. Update your password to `Complex.Password`.
5. Close the InPrivate or Incognito browser.

## Exercise 8 - Join the Domain

1. Once the ADConnect VM is successfully provisioned, connect to the ADConnect VM and logon as `ADConnectAdmin`.  Select **More Choices** then **Use a different account** to enter your credentials.
2. Click **No** on the **Networks** blade.
3. Within Server Manager, click **Don't show me this again** and close the **Windows Admin Center** window.
4. Click on **Local Server**, then **WORKGROUP**, then **Change** to rename this computer or join it to a domain.
5. Click the radio button for **Domain**, enter your fully-qualified domain name, such as `<myADdomain.TLD>`, and click **OK**.
6. In the Windows Security box enter the following:AD Domain Admin credentials:
    * username: **ADadmin**
    * password: `Complex.Password`
7. Click **Ok** on the Welcome screen, **Ok** on the Computer Name/Domain Changes window, **Close**, then **Restart Now**.

## Exercise 9 - Install Azure Active Directory Connect

1. Once the ADConnect VM has successfully rebooted, connect to the ADConnect VM and logon as `<yourADdomainname>\ADAdmin`, not `ADConnectAdmin` which is a local account. Select **More Choices** then **Use a different account** to enter your credentials.
    > Ensure that you are not logging on as local account!
2. When **Server Manager** opens select **Local Server** and turn off **IE Enhanced Security Configuration** for Administrators.
3. Open Internet Explorer, accept the defaults, and surf to [Microsoft Azure Active Directory Connect](http://go.microsoft.com/fwlink/?LinkId=615771).
4. Click **Download**, then **Run** when prompted.
Close Internet Explorer within the AD Connect virtual machine.

### Configure Azure Active Directory Connect

1. When installation start switch to the **Microsoft Azure Active Directory Connect** window and on the **Welcome to Azure AD Connect** screen select **I agree** then **Continue**.
2. Review the screen and select **Use express settings**.
3. On the **Connect to Azure AD** screen enter your **Azure AD Credentials**:

   * USERNAME: `AzureADSync@<yourAzureADDomain>.onmicrosoft.com`
   * PASSWORD: `Complex.Password`
   * Click **Next** and then confirm the credential are validated.  Correct any errors.
4. On the **Connect to AD DS screen**, enter the Active Directory Domain Services domain administrator credentials:
   * USERNAME: `<yourADDomain>\adadmin`
   * PASSWORD: `Complex.Password`
   * Click **Next** and then confirm the credential are validated.
5. On the **Azure AD sign-in configuration** screen, select the checkbox for **Continue without any verified domains** and click **Next**.

   > Since this is a temporary lab environment we are not going use a validated custom domain.
6. On the **Ready to Configure** screen click **Install**.
7. It may take 5-10 minutes for Azure AD Connect to complete installation. Read the **Configuration Complete** screen and then click **Exit**.
8. Minimize your RDP window.

### Validate Synchronization

1. Switch to the Azure portal and examine your Azure AD Directory by selecting the xxxx.onmicrosoft.com directory from the upper right hand corner of the portal.
2. Under **Manage** select **Users**. Note that you should now see accounts sourced from Windows Server AD that have synchronized to Azure Active Directory (e.g. Bob & Julia).

> Congratulations!  Your are now synchronizing Active Directory to Azure Active Directory!!
