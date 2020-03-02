# Windows Virtula Desktop Lab 1

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

We are using a VNET peering to simulate connecting your on premesis data center to a virtual network in Azure

### Peer AD-VNet to CloudDC-VNet

1. In the Search box at the top of the Azure portal, begin typing **vNet1**. When vNet1 appears in the search results, select it.
2. Select **Settings**, **Peerings**, and then click **+ Add**.
3. Enter the following information:
    * Name: **PeervNet1tovNet2**
    * Virtual network: **vNet2 (MyVNets)**
    * Name of the peering from vNet2 to vNet1: **PeervNet2tovNet1**  
    * Configure virtual network access settings: **Enabled**
    * Configure forwarded traffic settings: **Disabled**
    * Select **OK**
4. Peering status - If you don't see the status, refresh your browser.  Virtual network peering is not fully established until the peering status for both virtual networks is **Connected**.







Install and Configure Active Directory

In this task you use PowerShell within Windows Server 2019 to install Active Directory.

1. Once DC01 is running connect to the DC01 virtual machine and logon with your local account by selecting **Microsoft Azure / Resource Groups / CloudDC / CloudDC01 / Connect / RDP**.  
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