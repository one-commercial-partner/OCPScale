# Lab 2: Deploying Azure Infrastructure

In this exercise you will deploy the required infrastructure to provision a  Windows Virtual Desktop.

## Exercise 1 - Setup an IaaS Virtual Machine via Azure CLI

In this task you use the Azure CLI to create an Azure Virtual Machine running Windows Server 2019.

1. Open an Azure CLI window by browsing to [Azure Shell](https://shell.azure.com).
2. Login using your Microsoft Account.
3. When the **Welcome to Azure Cloud Shell** screen appears select **Bash** as the working CLI and then **Create Storage**.  Once storage is created click **Close**.
4. At the CLI prompt, let's create a new resource group to hold your Domain Controller. Create the resource group by typing in the following command:

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
    ```

8. Create a subnet:

    ```PowerShell
    az network vnet subnet create --address-prefix 10.10.10.0/24 --name AD-Subnet --resource-group WVDLab-Infrastructure --vnet-name AD-VNet --network-security-group AD-NSG
    ```

9. Create your virtual machine:.

    ```PowerShell
    az vm create --resource-group WVDLab-Infrastructure --name DC01 --size Standard_D2_v3 --image Win2019Datacenter --admin-username ADadmin --admin-password Complex.Password --nsg AD-NSG --private-ip-address 10.10.10.11
    ```

    > Write down your local credentials to DC01.

10. Close Azure Shell.

## Exercise 2 - Install and Configure Active Directory

In this task you use PowerShell (or PowerShell ISE) within Windows Server 2019 to install Active Directory.

1. Once DC01 is running connect to the DC01 virtual machine and logon with your local account (`ADadmin`) by selecting **Microsoft Azure / Resource Groups / WVDLab-Infrastructure / DC01 / Connect / RDP**.  
2. Make sure that you choose the **public IP address**, not the *Private IP address*, and then click on **Download RDP File**.
3. Logon with your local credentials that you wrote down earlier.  You may have to choose **More Choices** then **Use a different account** to enter your new set of credentials. The username is `ADadmin` and the password is `Complex.Password`.  Click the checkbox for **Don't ask me again for connections to this computer** and then **Yes** when prompted regarding the certificate error.
4. Once your desktop is running, click **No** on the Network Discovery blade.
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

## Exercise 3 - Connect to the Domain Controller and create a user account

1. Once DC01 has restarted connect to the virtual machine and logon with your domain account by selecting **Microsoft Azure / Resource Groups / WVDLab-Infrastructure / DC01 / Connect / RDP**.

2. Make sure that you choose the **public IP address**, not the `Private IP address`, and then click on **Download RDP File**.
3. Logon with the fully qualified domain credentials you wrote down earlier (e.g. `ADAdmin@<yourADdomain.TLD>`.  You may have to choose **More Choices** then **Use a different account** to enter your new set of credentials.

    > If you connected to the VM too quickly you will see the message "**Please wait for the Group Policy Client**" on your screen for several minutes.  Simply wait a few minutes for the desktop to render.

4. Within Server Manager, click **Don't show me this again** and close the **Windows Admin Center**.  Click **Tools** and then **Active Directory Users and Computers**.

5. Expand the domain tree and select **Users**. Right-click and select **New** then **User** and create a New User with the following information:
    * First Name: **WVD**
    * Last Name: **Administrator**
    * Full Name: **WVD Administrator**
    * User Logon Name: **wvdadmin**
6. Click **Next** and set the password to `Complex.Password`. Uncheck **User must change password at next logon**, and set the **Password never expires** checkbox. Click **Next** then **Finish**.

   > This account will be important in future tasks. Make a note of the username and password you create.

7. In Active Directory Users and Computers, select and then right-click on the `WVD Administrator` account object and select **Add to a group**.

8. On the Select Groups dialog window, type **Enterprise Admins**, click **Check Names**, and then click **OK**.  Click **OK** once the operation is completed successfully.

   > **Note:** This account will be used during the host pool creation process for joining the hosts to the domain. Granting Enterprise Admin permissions will simplify the lab, however we recommend implementing RBAC for production environments.

### Create Domain Accounts

1. In the domain tree select **Users**, then right-click, **New**  then **User** and create a user object with the following information:
    * First Name: **Bob**
    * Last Name: **Jones**
    * Full Name: **Bob Jones**
    * User Logon Name: **Bob.Jones**
2. Click **Next** and set the password to `Complex.Password`. Uncheck **User must change password at next logon**, and set the **Password never expires** checkbox.
3. Click **Next** then **Finish**.
4. In the domain tree select **Users**, then right-click, **New**  then **User** and create a user object with the following information:
    * First Name: **Julia**
    * Last Name: **Williams**
    * Full Name: **Julia Williams**
    * User Logon Name: **julia.williams**
5. Click **Next** and set the password to `Complex.Password`. Uncheck **User must change password at next logon**, and set the **Password never expires** checkbox.
6. Click **Next** then **Finish**.

## Exercise 4 - Configure DNS

The virtual network that contains the domain controller is pointing to Azure DNS, not the DNS of the domin controller, for name resolution.  Any new VMs will not be able to find the DNS service on the domain controller and be able to join the domain.  Change the DNS to point to the domain controller.

1. In the Azure portal click **Home** -> **Resource groups** -> **WVDLab-Infrastructure**.
2. Click on **DC01** and copy the Private IP address (e.g. 10.10.10.11).
3. Click on **WVDLab-Infrastructure** and then **AD-Vnet**.
4. Under **Settings** click **DNS Servers**.
5. Change the DNS servers to **Custom** and paste the IP address.
6. Click **Save**.

## Exercise 5 - Create a virtual machine to host AD Connect

Following [Microsoft recommended practices](https://docs.microsoft.com/en-us/azure/active-directory/hybrid/how-to-connect-install-prerequisites#azure-ad-connect-server) we are creating a virtual machine to be used to host Azure AD Connect.

1. From your desktop return to and open an Azure CLI window by browsing to [Azure Shell](https://shell.azure.com).
2. You may have to hit **Reconnect**.
3. Create your virtual machine:

    ```PowerShell
    az vm create --resource-group WVDLab-Infrastructure --name ADConnect --size Standard_D2_v3 --image Win2019Datacenter --admin-username ADConnectAdmin --admin-password Complex.Password --nsg AD-NSG --private-ip-address 10.10.10.15
    ```

    > It will take about 5 minutes to provision this VM, so please move to Exercise 5.

## Exercise 6 - Join the Domain

1. Once the ADConnect VM is successfully started, connect to the ADConnect VM and logon as `ADConnectAdmin`.  Select **More Choices** then **Use a different account** to enter your credentials.
2. Click **No** on the **Networks** blade.
3. Within Server Manager, click **Don't show me this again** and close the **Windows Admin Center** window.
4. Click on **Local Server**, then **WORKGROUP**, then **Change** to rename this computer or join it to a domain.
5. Click the radio button for **Domain**, enter your fully-qualified domain name, such as `<myADdomain.TLD>`, and click **OK**.
6. In the Windows Security box enter the following:AD Domain Admin credentials:
    * username: **ADadmin**
    * password: `Complex.Password`
7. Click **Ok** on the Welcome screen, **Ok** on the Computer Name/Domain Changes window, **Close**, then **Restart Now**.

### Continue with Lab 3: [Configuring Azure AD Connect with AD-DS](Prepare-Lab03-Configuring-Azure-AD-Connect-with-AD-DS.md)

### Return to [Prepare Phase Labs](prepare.md)
