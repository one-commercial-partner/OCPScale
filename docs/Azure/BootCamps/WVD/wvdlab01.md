# Windows Virtual Desktop Lab 1

In this lab, and just like you would in the real world, you will first establish connectivity with your on-premisis data center with your resources in Azure.  Next, you will synchronize an on-premesis domain controller to an IaaS virtual machine running as a domain controller in Azure. 

## Pre-requisites

Please complete the [Hybrid Identity Lab](wvdhomework.md) prior to starting this lab.

## Exercise 1 - Setup an IaaS Virtual Machine via Azure CLI

In this task you use the Azure CLI to create an Azure Virtual Machine running Windows Server 2019.

1. Open an Azure CLI window by browsing to [Azure Shell](https://shell.azure.com). Login using your Microsoft Account if needed.
2. At the CLI prompt, let's create a new resource group to hold your Domain Controller VMs. Create the resource group by typing in the following command:

    `az group create --name CloudDC --location eastus`

3. Create a network security group:

    `az network nsg create --name CloudDC-NSG --resource-group CloudDC --location eastus`

4. Create a network security group rule for port 3389:

    `az network nsg rule create --name PermitRDP --nsg-name CloudDC-NSG --priority 1000 --resource-group CloudDC --access Allow --source-address-prefixes "*" --source-port-ranges "*" --direction Inbound --destination-port-ranges 3389`

5. Create a virtual network:

    `az network vnet create --name CloudDC-VNet --resource-group CloudDC --address-prefixes 10.100.0.0/16 --location eastus`

6. Create a subnet:

    `az network vnet subnet create --address-prefix 10.100.10.0/24 --name CloudDC-Subnet --resource-group CloudDC --vnet-name CloudDC-VNet --network-security-group CloudDC-NSG`

7. Create an availability set.  You want to keep your domain controllers resilient.

    `az vm availability-set create --name CloudDC-AvailabilitySet --resource-group CloudDC --location eastus`

8. Create your virtual machine, noting to change the value of **--admin-username** before executing the script.

    `az vm create --resource-group CloudDC --availability-set CloudDC-AvailabilitySet --name CloudDC01 --size Standard_D2_v3 --image Win2019Datacenter --admin-username *yourfirstname* --admin-password Complex.Password --nsg CloudDC-NSG --private-ip-address 10.100.10.11 --no-wait`

At this point please write down the local credentials you just created.

## Exercise 2 - Peer your Vnets

You can connect virtual networks to each other with virtual network peering. These virtual networks can be in the same region or different regions (also known as Global VNet peering). Once virtual networks are peered, resources in both virtual networks are able to communicate with each other, with the same latency and bandwidth as if the resources were in the same virtual network.

We are using a VNET peering to simulate connecting your on premesis data center to a virtual network in Azure.

### Peer AD-VNet to CloudDC-VNet

1. In the Search box at the top of the Azure portal, begin typing **AD-VNet**. When AD-VNet appears in the search results, select it.
2. Select **Settings**, **Peerings**, and then click **+ Add**.
3. Enter the following information:
    * Name: **PeerGroundtoCloud**
    * Virtual network: **CloudDC-VNet (CloudDC)**
    * Name of the peering from vNet2 to vNet1: **PeerCloudtoGround**  
    * Configure virtual network access settings: **Enabled**
    * Configure forwarded traffic settings: **Disabled**
    * Select **OK**
4. Peering status - hit **refresh** in the Azure portal.  Virtual network peering is not fully established until the peering status for both virtual networks is **Connected**.

### Exercise 3 - Join the CloudDC VM to the domain

1. Once the cloud shell has built your VM, connect to the **CloudDC** virtual machine and logon. **Microsoft Azure / Resource Groups / CloudDC / CloudDC01 / Connect / RDP**.  Make sure that you choose the **public IP address**, not the `Private IP address`, and then click on **Download RDP File**.
2. Logon with local credentials you created earlier.  Choose **More Choices** then **Use a different account** to enter your new set of credentials.
3. When prompted click **No** on the Network discovery blade.
4. The DNS Server on ADConnect may not be set to see the domain controller (DC01), so we need to check that setting.  
5. Open a **Command prompt** (**Start Button** -> **Windows System**) and enter *ipconfig /all*.  If the DNS Server is set to 10.10.10.11 (the private IP address of DC01), close the Command Prompt window and then continue to **Join the Domain**, otherwise proceed to the **Configure DNS** set of tasks.

### Configure DNS

1. Within **Server Manager**, click on **Local Server**.
2. Click on **IPv4 address assigned by DHCP, IPv6 enabled** setting for the Ethernet connection.
3. Right-click on the network adapter and choose **Properties**.
4. Select **Internet Protocol Version 4 (TCP/IPv4)** and then **Properties**.
5. Select the radio button for **Use the following DNS Server addresses:** and Set the DNS server to **10.10.10.11** and click **OK** and then **Close**.
6. You will then lose connection to the CloudDC01 VM, this is expected. Once you are back at the Microsoft Azure Portal, click **Restart** to restart the CloudDC01 VM.

### Join the Domain

1. Once the CloudDC01 VM is successfully restarted, connect to the virtual machine with your previously created credentials.  Within **Server Manager**, click on **Local Server**.
2. Click on **WORKGROUP**, then **Change** to rename this computer or join it to a domain.
3. Click the radio button for **Domain**, enter your fully-qualified domain name you specified earlier and click **OK**.
4. In the Windows Security box enter the AD Domain Admin credentials you specified earlier.
5. Click **Ok** on the Welcome screen, **Ok** on the Computer Name/Domain Changes window, **Close**, then **Restart Now**.

## Exercise 4 - Install and Configure Active Directory

In this task you use PowerShell within Windows Server 2019 to install Active Directory.

1. Once CloudDC01 is running connect to the virtual machine and logon with your local account by selecting **Microsoft Azure / Resource Groups / CloudDC / CloudDC01 / Connect / RDP**.  
2. Make sure that you choose the **public IP address**, not the *Private IP address*, and then click on **Download RDP File**.
3. Logon with your local credentials that you wrote down earlier.  You may have to choose **More Choices** then **Use a different account** to enter your new set of credentials.
4. If prompted click **No** on the Network Discovery blade.
5. Hit the **Windows Start** button and then open **Windows PowerShell**. Enter the following to install the Active Directory Domain Service module:

    `install-windowsfeature AD-Domain-Services -IncludeAllSubFeature -IncludeManagementTools`
6. Import the deployment modules by entering the following:

    `Import-Module ADDSDeployment`

> *Note that PowerShell will quickly return as this command takes milliseconds to execute.*

7. Promote your server to a domain controller by entering the following command.  Don't forget to set the **domain names properly** minding the quotes.

    `Install-ADDSDomainController -Credential (Get-Credential) -DatabasePath "C:\Windows\NTDS”  -DomainName “YOURDOMAIN.COM” -InstallDns:$true
-LogPath “C:\Windows\NTDS” -SysvolPath "C:\Windows\SYSVOL” -Force:$true`

8. Once you hit enter you will be asked for credentials.  Enter the domain credentials previously supplied (domainname\username).  Also, enter `Complex.Password` for the SafeModeAdministratorPassword, and then retype to confirm.

9. Once Active Directory is installed your virtual machine will restart.
