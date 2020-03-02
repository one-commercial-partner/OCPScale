# Hybrid Identity Homework

## Before you Begin

If you are using a Azure subscription that was provided to you by Microsoft, you are limited to a specific set of Microsoft Azure regions that you can use. Please use any of the following otherwise you will receive an  error in the portal if you select an unsupported region and attempt to build anything in Microsoft Azure.

* **East US**
* **South Central US**
* **West US 2**
* **West Central**

## Exercise 1 - Setup an IaaS Virtual Machine via Azure CLI

In this task you use the Azure CLI to create an Azure Virtual Machine running Windows Server 2019.

1. Open an Azure CLI window by browsing to [Azure Shell](https://shell.azure.com).
2. Login using your Microsoft Account.
3. When the **Welcome to Azure Cloud Shell** screen appears select **Bash** as the working CLI and then select **Create Storage**.  Once storage is created your cloud shell will provision.
4. At the CLI prompt, let's create a new resource group to hold your Domain Controller VMs. Create the resource group by typing in the following command:

    `az group create --name OnPremDC --location eastus`

5. Create a network security group:

    `az network nsg create --name AD-NSG --resource-group OnPremDC --location eastus`

6. Create a network security group rule for port 3389:

    `az network nsg rule create --name PermitRDP --nsg-name AD-NSG --priority 1000 --resource-group OnPremDC --access Allow --source-address-prefixes "*" --source-port-ranges "*" --direction Inbound --destination-port-ranges 3389`

7. Create a virtual network:

    `az network vnet create --name AD-VNet --resource-group OnPremDC --address-prefixes 10.10.0.0/16 --location eastus`

8. Create a subnet:

    `az network vnet subnet create --address-prefix 10.10.10.0/24 --name AD-Subnet --resource-group OnPremDC --vnet-name AD-VNet --network-security-group AD-NSG`

9. Create an availability set.  You want to keep your domain controllers resilient.

    `az vm availability-set create --name AD-AvailabilitySet --resource-group OnPremDC --location eastus`

10. Create your virtual machine, noting to change the value of **--admin-username** before executing the script.

    `az vm create --resource-group OnPremDC --availability-set AD-AvailabilitySet --name DC01 --size Standard_D2_v3 --image Win2019Datacenter --admin-username *yourfirstname* --admin-password Complex.Password --nsg AD-NSG --private-ip-address 10.10.10.11 --no-wait`

At this point please write down the local credentials you just created.

## Exercise 2 - Install and Configure Active Directory

In this task you use PowerShell within Windows Server 2019 to install Active Directory.

1. Once DC01 is running connect to the DC01 virtual machine and logon with your local account by selecting **Microsoft Azure / Resource Groups / OnPremDC / DC01 / Connect / RDP**.  
2. Make sure that you choose the **public IP address**, not the *Private IP address*, and then click on **Download RDP File**.
3. Logon with your local credentials that you wrote down earlier.  You may have to choose **More Choices** then **Use a different account** to enter your new set of credentials.
4. When prompted click **No** on the Network Discovery blade.
5. Hit the **Windows Start** button and then open **Windows PowerShell**. Enter the following to install the Active Directory Domain Service module:

    `install-windowsfeature AD-Domain-Services -IncludeAllSubFeature -IncludeManagementTools`
6. Import the deployment modules by entering the following:

    `Import-Module ADDSDeployment`

> *Note that PowerShell will quickly return as this command takes milliseconds to execute.*

7. Promote your server to a domain controller by entering the following command.  Don't forget to set the **domain names properly** minding the quotes.

    `Install-ADDSForest -CreateDnsDelegation:$false -DatabasePath "C:\Windows\NTDS” -DomainMode “Win2012R2” -DomainName “YOURDOMAIN.COM”
-DomainNetbiosName “YOURDOMAIN” -ForestMode “Win2012R2” -InstallDns:$true
-LogPath “C:\Windows\NTDS” -SysvolPath "C:\Windows\SYSVOL” -Force:$true`

> *Write down your FQDN doman name and NETBIOS domain name for future reference.*

8. Once you hit enter you will be asked for the  SafeModeAdministratorPassword – this is for the Directory Services Restore Mode (DSRM). Enter `Complex.Password`, and then retype to confirm.

9. Once Active Directory is installed your virtual machine will restart.

## Exercise 3 - Connect to the Domain Controller and create a user account

1. Once DC01 has restarted connect to the virtual machine by selecting **Microsoft Azure / Resource Groups / AD-ResourceGroup / DC01 / Connect / RDP**.

2. Make sure that you choose the **public IP address**, not the `Private IP address`, and then click on **Download RDP File**.
3. Logon with the fully qualified domain credentials you wrote down earlier (e.g. yourname@yourdomain.com).  You may have to choose __More Choices__ then **Use a different account** to enter your new set of credentials.

> *Note that if you connected to the VM too quickly you will see the message "**Please wait for the Group Policy Client**" on your screen for several minutes.*

4. Within Server Manager, click **Tools** and then **Active Directory Users and Computers**.
5. Expand the tree and select the **Users** Container.
6. On the toolbar click the icon to create a new user in the current container.  
7. Create a New User with the following information:
    * First Name: **On**
    * Last Name: **Prem**
    * Full Name: **On Prem**
    * User Logon Name: **onprem**
8. Click **Next** and set the password to `Complex.Password`. Uncheck **User must change password at next logon**, and set the **Password never expires** checkbox.
9. Click **Next** then **Finish**.
10. Minimize the RDP window.

## Exercise 4 - Create a virtual machine to host AD Connect

We are creating a small VM to be used later to host Azure AD Connect.

1. Open an Azure CLI window by browsing to [Azure Shell](https://shell.azure.com) and login using your Microsoft Account.
2. Create an availability set.  You want to keep your virtual machines  resilient.

    `az vm availability-set create --name ADConnect-AvailabilitySet --resource-group OnPremDC --location eastus`

3. Create your virtual machine:

    `az vm create --resource-group OnPremDC --availability-set ADConnect-AvailabilitySet --name ADConnect --size Standard_D2_v3 --image Win2019Datacenter --admin-username ADAdmin --admin-password Complex.Password --nsg AD-NSG --private-ip-address 10.10.10.15`

## Exercise 5 - Join the ADConnect VM to the domain

1. Once the cloud shell has built your VM, connect to the **ADConnect** virtual machine and logon. **Microsoft Azure / Resource Groups / AD-ResourceGroup / ADConnect / Connect / RDP**.  Make sure that you choose the **public IP address**, not the `Private IP address`, and then click on **Download RDP File**.
2. Logon with local credentials (i.e. ADAdmin) with a password of `Complex.Password`.  Choose **More Choices** then **Use a different account** to enter your new set of credentials.
3. When prompted click **No** on the Network discovery blade.
4. The DNS Server on ADConnect may not be set to see the domain controller (DC01), so we need to check that setting.  
5. Open a **Command prompt** (**Start Button** -> **Windows System**) and enter *ipconfig /all*.  If the DNS Server is set to 10.10.10.11 (the private IP address of DC01), close the Command Prompt window and then continue to **Exercise 6 - Join the Domain**, otherwise proceed to the **Configure DNS** set of tasks.

### Configure DNS

1. Within **Server Manager**, click on **Local Server**.
2. Click on **IPv4 address assigned by DHCP, IPv6 enabled** setting for the Ethernet connection.
3. Right-click on the network adapter and choose **Properties**.
4. Select **Internet Protocol Version 4 (TCP/IPv4)** and then **Properties**.
5. Select the radio button for **Use the following DNS Server addresses:** and Set the DNS server to **10.10.10.11** and click **OK** and then **Close**.
6. You will then lose connection to the ADConnect VM, this is expected. Once you are back at the Microsoft Azure Portal, click **Restart** to restart the ADConnect VM.

## Exercise 6 - Join the Domain

1. Once the ADConnect VM is successfully restarted, connect to the ADConnect VM and logon as ADAdmin.  Within **Server Manager**, click on **Local Server**.
2. Click on **WORKGROUP**, then **Change** to rename this computer or join it to a domain.
3. Click the radio button for **Domain**, enter your fully-qualified domain name, such as mydomainname.com, and click **OK**.
4. In the Windows Security box enter the AD Domain Admin credentials you specified earlier.
5. Click **Ok** on the Welcome screen, **Ok** on the Computer Name/Domain Changes window, **Close**, then **Restart Now**.

## Exercise 7 - Install Azure Active Directory

1. In the Azure Portal, click **Microsoft Azure** and then **+Create a resource**.  Select **Identity** and then **Azure Active Directory**.
2. Enter the following on the **Create directory tab**:
    * Organization name (e.g. *yourfirstname* Mike's Org)
    * Initial domain name (e.g. your initials plus last four of your cellphone).  Hit **Tab**.

> *Ensure validation passes as your namespace needs to be unique within the onmicrosoft.com namespace.  We often see students choosing a domain name that already exists.*

        ***Write this domain name down as your Azure Active Directory Domain Name.***
3. Click **Create**.  It will take several minutes for the directory to be created.
4. Once complete, select Click **here** to manage your new directory.

## Exercise 8 - Create a Sync Account

We are going to create an account that AD Connect will use to perform the synchronization process bethween the on-prem domain controller and Azure Active Directory.

1. In Azure Active Directory, under **Manage** choose **Users** and then under **All users** click on **+New User** and enter the following:
    * User name: **adsync**
    * Name: **AD Sync Account**
    * Click on **Show Password** and then copy the password.
    * Under **Roles** click **User**.  Search for and select a directory role named: **Global administrator**, then click **Select**.
2. Click **Create**.
3. Open an InPrivate or Incognito browser and surf to <https://portal.azure.com.>
4. Login as the AD Sync Account you just created using the temporary password.
5. Change your password to `Complex.Password` and then click **Sign in**.
6. Close your inprivate or incognito browser.

## Exercise 9 - Install Azure Active Directory Connect

1. Connect to the ADConnect VM and logon as your previously created **domain account** (i.e. `domainname\username`, not adadmin which is a local account).  If you don’t see the VM, you might need to  switch from the new Azure Active Directory you just created to the **Default Directory** associated with your subscription.  Click in the upper right-hand corner of the screen to change directories.
2. When **Server Manager** opens select **Local Server** and turn off **IE Enhanced Security Configuration** for Administrators and Users.
3. Open Internet Explorer, accept the defaults, and surf to <http://go.microsoft.com/fwlink/?LinkId=615771>
4. Click **Download**, then **Run** when prompted.
Close Internet Explorer.

## Exercise 10 -  Configure Azure Active Directory Connect

1. On the Welcome to Azure AD Connect screen select **I agree** then **Continue**.
2. Review the screen and select **Use express settings**.
3. On the **Connect to Azure AD** screen enter your **Azure AD Credentials**.  This would be the *adsync@yourdirectoryname.onmicrosoft.com*  account you created.  Click **Next** and then confirm the credential are validated.
4. On the **Connect to AD DS screen**, enter the Active Directory domain administrator credentials. This would be the account you created in the original template (i.e. mydomain\myusername). Click **Next** and confirm the credential are validated.  

> If you get an error about the current security context is not associated with an Active Directory domain or forest, you more than likely didn’t logon with a domain account but rather a local account.  You can verify this by opening a command prompt and entering **whoami**.  Logout and login with a domain account and then restart at step 1 in this section.

5. On the **Azure AD sign-in configuration** screen, select the checkbox for **Continue without any verified domains** and click **Next**.

> Since this is a temporary lab environment we are not going use a validated custom domain.

6. On the **Ready to Configure** screen click **Install**.
7. It may take 5-10 minutes for Azure AD Connect to complete installation. Read the **Configuration Complete** screen and then click **Exit**.
8. Minimize your RDP window.

## Exercise 11 - Validate Synchronization

1. Switch to the Azure portal and examine your Azure AD Directory by selecting the xxxx.onmicrosoft.com  Directory from the upper right hand corner of the portal.
2. Under **Manage** select **Users**. Note that you should now see accounts sourced from Windows Server AD that have synchronized to Azure Active Directory (e.g. On Prem).

### Congratulations!  Your are now synchronizing Active Directory to Azure Active Directory
