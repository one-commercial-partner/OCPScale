# Lab 2: Deploying Azure Infrastructure

In this exercise you will deploy the required infrastructure to provision a  Windows Virtual Desktop.

## Exercise 1 - Setup an IaaS Virtual Machine via Azure CLI

In this task you use the Azure CLI to create an Azure Virtual Machine running Windows Server 2019.

1. Open an Azure CLI window by browsing to [Azure Shell](https://shell.azure.com).
2. Login using your Microsoft Account.
3. When the **Welcome to Azure Cloud Shell** screen appears select **Bash** as the working CLI and then **Create Storage**.  Once storage is created click **Close**.
4. At the CLI prompt, let's create a new resource group to hold your Domain Controller VMs. Create the resource group by typing in the following command:

    ```PowerShell
    az group create --name WVDLab-Infrastructure --location eastus
    ```

5. Create a network security group:

    ```PowerShell
    az network nsg create --name AD-NSG --resource-group WVDLab-Infrastructure --location eastus
    ```

6. Create a network security group rule for port 3389.

    ```PowerShell
    az network nsg rule create --name PermitRDP --nsg-name AD-NSG --priority 1000 --resource-group WVDLab-Infrastructure --access Allow --source-address-prefixes "*" --source-port-ranges "*" --direction Inbound --destination-port-ranges 3389
    ```

7. Create a virtual network.

    ```PowerShell
    az network vnet create --name AD-VNet --resource-group WVDLab-Infrastructure --address-prefixes 10.10.0.0/16 --location eastus

8. Create a subnet:

    ```PowerShell
    az network vnet subnet create --address-prefix 10.10.10.0/24 --name AD-Subnet --resource-group WVDLab-Infrastructure --vnet-name AD-VNet --network-security-group AD-NSG
    ```

9. Create your virtual machine:.

    ```PowerShell
    az vm create --resource-group WVDLab-Infrastructure --name DC01 --size Standard_D2_v3 --image Win2019Datacenter --admin-username adadmin --admin-password Complex.Password --nsg AD-NSG --private-ip-address 10.10.10.11
    ```

## Exercise 2 - Install and Configure Active Directory

In this task you use PowerShell (or PowerSHell ISE) within Windows Server 2019 to install Active Directory.

1. Once DC01 is running connect to the DC01 virtual machine and logon with your local account by selecting **Microsoft Azure / Resource Groups / WVDLab-Infrastructure / DC01 / Connect / RDP**.  
2. Make sure that you choose the **public IP address**, not the *Private IP address*, and then click on **Download RDP File**.
3. Logon with your local credentials that you wrote down earlier.  You may have to choose **More Choices** then **Use a different account** to enter your new set of credentials. The username is `adadmin` and the password is `Complex.Password`.  Click the checkbox for **Don't ask me again for connections to this computer** and then **Yes** when prompted regarding the certificate error.
4. When prompted click **No** on the Network Discovery blade.
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
    Install-ADDSForest -CreateDnsDelegation:$false -DatabasePath "C:\Windows\NTDS” -DomainMode “Win2012R2” -DomainName “YOURDOMAIN.COM”
    -DomainNetbiosName “YOURDOMAIN” -ForestMode “Win2012R2” -InstallDns:$true
    -LogPath “C:\Windows\NTDS” -SysvolPath "C:\Windows\SYSVOL” -Force:$true
    ```

    *Write down your FQDN doman name for future reference.*

8. Once you hit enter you will be asked for the  SafeModeAdministratorPassword – this is for the Directory Services Restore Mode (DSRM). Enter `Complex.Password`, and then retype to confirm.

    > You will receive warnings about security settings, network adapters, and DNS Servers.  These warnings can be ignored.

9. Once Active Directory is installed your virtual machine will restart.

## Exercise 3 - Connect to the Domain Controller and create a user account

By default, Azure AD Connect does not synchronize the built-in domain administrator account *ADAdmin\@MyADDomain.com*. This system account has the attribute `isCriticalSystemObject` set to *true*, preventing it from being synchronized. While it is possible to modify this, it is not a best practice to do so.

1. Once DC01 has restarted connect to the virtual machine and logon with your domain account by selecting **Microsoft Azure / Resource Groups / WVDLab-Infrastructure / DC01 / Connect / RDP**.

2. Make sure that you choose the **public IP address**, not the `Private IP address`, and then click on **Download RDP File**.
3. Logon with the fully qualified domain credentials you wrote down earlier (e.g. adadmin@yourdomain.com).  You may have to choose **More Choices** then **Use a different account** to enter your new set of credentials.

    > If you connected to the VM too quickly you will see the message "**Please wait for the Group Policy Client**" on your screen for several minutes.
    
    > When you connect with RDP you will see adadmin as the default credentials.. These are local crdentials, not domain credentials, so be sure to click on **More choices** then **Use a different account** and enter FQDN domain credentials.

4. Within Server Manager, click **Tools** and then **Active Directory Users and Computers**.

5. Expand the domain tree and select **Users**. Right-click and create a New User with the following information:
    * First Name: **WVD**
    * Last Name: **Administrator**
    * Full Name: **WVD Administrator**
    * User Logon Name: **wvdadmin**
6. Click **Next** and set the password to `Complex.Password`. Uncheck **User must change password at next logon**, and set the **Password never expires** checkbox.
7. Click **Next** then **Finish**.

   > *Tip:* This account will be important in future tasks. Make a note of the username and password you create.

8. In Active Directory Users and Computers, select and then right-click on the `WVD Administrator` account object and select **Add to a group**.

9. On the Select Groups dialog window, type **Enterprise Admins**, click **Check Names**, and then click **OK**.  Click **OK** once the operation is completed successfully.

   > **Note:** This account will be used during the host pool creation process for joining the hosts to the domain. Granting Enterprise Admin permissions will simplify the lab. However, any Active Directory account that has the following permissions will suffice. This can be done using [Active Directory Delegate Control.](https://danielengberg.com/domain-join-permissions-delegate-active-directory/)

### Create Domain Accounts

1. Right-click and create a New User with the following information:
    * First Name: **Bob**
    * Last Name: **Jones**
    * Full Name: **Bob Jones**
    * User Logon Name: **Bob.Jones**
2. Click **Next** and set the password to `Complex.Password`. Uncheck **User must change password at next logon**, and set the **Password never expires** checkbox.
3. Click **Next** then **Finish**.
4. Right-click and create a New User with the following information:
    * First Name: **Julia**
    * Last Name: **Williams**
    * Full Name: **Julia Williams**
    * User Logon Name: **jullia.williams**
5. Click **Next** and set the password to `Complex.Password`. Uncheck **User must change password at next logon**, and set the **Password never expires** checkbox.
6. Click **Next** then **Finish**.

## Exercise 4 - Create a virtual machine to host AD Connect

We are creating a small VM to be used later to host Azure AD Connect.

1. From your desktop return to and open an Azure CLI window by browsing to [Azure Shell](https://shell.azure.com).
2. You mau have to hit **Reconnect**.
3. Create your virtual machine:

    ```PowerShell
    az vm create --resource-group WVDLab-Infrastructure --name ADConnect --size Standard_D2_v3 --image Win2019Datacenter --admin-username LocalAdmin --admin-password Complex.Password --nsg AD-NSG --private-ip-address 10.10.10.15
    ```

    > It will take about 5 minutes to provision this VM.

## Exercise 5 - Join the ADConnect VM to the domain

1. Once the cloud shell has built your ADConnect VM, connect to the **ADConnect** virtual machine and logon. **Microsoft Azure / Resource Groups / WVDLab-Infrastructure / ADConnect / Connect / RDP**.  Make sure that you choose the **public IP address**, not the `Private IP address`, and then click on **Download RDP File**.
2. Logon with local credentials (i.e. LocalAdmin) with a password of `Complex.Password`.  Choose **More Choices** then **Use a different account** to enter your new set of credentials.  Click the checkbox for **Don't ask me again for connections to this computer** and then **Yes** when prompted regarding the certificate error.
3. When prompted click **No** on the Network discovery blade.
4. The DNS Server on ADConnect may not be set to see the domain controller (DC01), so we need to check that setting.  
5. Open a **Command prompt** (**Start Button** -> **Windows System**) and enter *ipconfig /all*.  If the DNS Server is set to 10.10.10.11 (the private IP address of DC01), close the Command Prompt window and then continue to **Exercise 6 - Join the Domain**, otherwise proceed to the **Configure DNS** set of tasks.

### Configure DNS

1. Within **Server Manager**, close the message about **Windows Admin Center** and then click on **Local Server**.
2. Click on **IPv4 address assigned by DHCP, IPv6 enabled** setting for the Ethernet connection.
3. Right-click on the network adapter and choose **Properties**.
4. Select **Internet Protocol Version 4 (TCP/IPv4)** and then **Properties**.
5. Select the radio button for **Use the following DNS Server addresses:** and Set the DNS server to **10.10.10.11** and click **OK** and then **Close**.
6. You will then lose connection to the ADConnect VM, this is expected.
7. Once you are back at the Microsoft Azure Portal, click **Restart** to restart the **ADConnect** VM.

## Exercise 6 - Join the Domain

1. Once the ADConnect VM is successfully restarted, connect to the ADConnect VM and logon as LocalAdmin.  Within **Server Manager**, click on **Local Server**.
2. Click on **WORKGROUP**, then **Change** to rename this computer or join it to a domain.
3. Click the radio button for **Domain**, enter your fully-qualified domain name, such as mydomainname.com, and click **OK**.
4. In the Windows Security box enter the following:AD Domain Admin credentials:
    * username: **adadmin**
    * password: `Complex.Password`
5. Click **Ok** on the Welcome screen, **Ok** on the Computer Name/Domain Changes window, **Close**, then **Restart Now**.

### Continue with Lab 3: [Configuring Azure AD Connect with AD-DS](Prepare-Lab03-Configuring-Azure-AD-Connect-with-AD-DS.md)

### Return to [Prepare Phase Labs](prepare.md)
