# Lab 1 - Virtual Networks

## Before you Begin

If you are using a Microsoft Azure subscription that was provided to you by Microsoft, you using what is called sponsored Azure and that subscription is  limited to a specific set of Microsoft Azure regions. Please consistently use one of the following locations:

* East US
* South Central US
* West Europe
* Southeast Asia
* West US 2
* West Central US

Otherwise you will receive an error in the portal if you select an unsupported region and attempt to build anything in Microsoft Azure.

Additionally, your Azure subscription is limited in the amount of cores that you can provision.  Ensure that you have deleted the following VMs before completing this lab:

* ADConnect2 and ADConnect3

## Lab Summary

In this lab you are going to create multiple virtual networks each with it's own virtual machine and subnet and then test connectivity across subnets and vnets.

## Exercise 1 -  Create three virtual networks

### vNet1

1. Log in to the Azure portal at <https://portal.azure.com> and click on **+Create a resource**  on the upper left corner of the Azure portal.
2. Select **Networking**, and then select **Virtual network**.
3. Enter or select the following information, accept the defaults for the remaining settings, and then click **Create**:
    * Name: **vNet1**
    * Address Space: **10.101.0.0/16**
    * Resource Group: *Create New* **VNets**
    * Location: *Choose a consistent and supported location*
    * Subnet Name: **subnet1**
    * Subnet address range: **10.101.1.0/24**

### vNet2

Repeat the steps above for vNet2:

* Name: **vNet2**
* Address Space: **10.102.0.0/16**
* Resource Group: **VNets**
* Location: *Choose a consistent and supported location*
* Subnet Name: **subnet2**
* Subnet address range: **10.102.2.0/24**

### vNet3

Repeat the steps above for vNet3:

* Name: **vNet3**
* Address Space: **10.103.0.0/16**
* Resource Group: **VNets**
* Location: *Choose a consistent and supported location*
* Subnet Name: **subnet3**
* Subnet address range: **10.103.3.0/24**

## Exercise 2 - Create three virtual machines

1. Return to the Azure portal and click the **+Create a Resource** button found on the upper left-hand corner of the Azure portal.
2. Select **Compute** then select **Virtual machine**.
3. On the Basics tab complete the following:
    * Resource Group:  *Create New* **NetVMs**
    * Virtual machine name: **VM1**
    * Region: Choose the same region as your other resources
    * Availability options: No infrastructure redundancy required
    * Image: Windows Server 2016 Datacenter
    * Size: Choose **DS1_v2**
    * Username: *pickaname* and write it down
    * Password: `Complex.Password`
    * Confirm Password: `Complex.Password`
    * Public inbound ports: **Allow selected ports**
    * Select inbound ports: **RDP (3389)**
4. Click **Next:Disks >** and then **Next:Networking >**.
5. Select **vNet1** for the Virtual network.
6. **Review + create** and then **Create**.   After validation passes, monitor your deployment status. It should take less than 10 minutes to spin up the VM.

### Create the second VM

1. Return to the Azure portal and click the **Create a Resource** button (the Plus) found on the upper left-hand corner of the Azure portal.
2. Select **Compute** then select **Virtual machine**.
3. On the Basics tab complete the following:
    * Resource Group:  **NetVMs**
    * Virtual machine name: **VM2**
    * Region: Choose the same region as your other resources
    * Availability options: No infrastructure redundancy required
    * Image: Windows Server 2016 Datacenter
    * Size: Choose **DS1_v2**
    * Username: *pickaname* and write it down
    * Password: `Complex.Password`
    * Confirm Password: `Complex.Password`
    * Public inbound ports: **Allow selected ports**
    * Select inbound ports: **RDP (3389)**
4. Click the **Networking** tab.
5. Select **vNet2** for the Virtual network.
6. **Review + create**.   After validation passes, click  **Create** and then monitor your deployment status. It should take less than 10 minutes to spin up the VM.

## Exercise 4 - OPTIONAL - Create the third VM

We are going to create the third VM using PowerShell within Cloud Shell.  Only create this VM if you plan on completing the Hub and SPoke Challenge.

1. Click on the Cloud Shell icon on the taskbar: **>_**
2. Select **PowerShell**.
3. If you are prompted, select **Create Storage**.
4. Enter the following to set the username and password needed for the administrator account on the VM :
    `$cred = Get-Credential`
5. Create the VM (note to use the correct region):
    `New-AzVm
    -ResourceGroupName "NetVMs"
    -Name "VM3"
    -Location "EastUS"
    -VirtualNetworkName "vNet3"
    -SubnetName "Subnet3"
    -SecurityGroupName "VM3-nsg"
    -PublicIpAddressName "VM3-ip"
    -Credential $cred`

You now have three virtuals machines each in their own virtual network and subnet. Let's validate that.

1. Click on **Network Watcher** from the left hand pane of the Azure Portal.
2. Under  **Monitoring** choose **Topology**.
3. Under **Resource Group** select **VNets**.  In a moment a conceptual network diagram should be generated showing all three vNets and subnets.  Notice that there is no link between vNet1, vNet2, or vNet3.

## Exercise 5 - Connect to a VM and test connectivity

Before you begin this section, obtain the private and public IP addresses of VM1, VM2, and VM3.

1. At the top of the Azure portal in the Search bar enter **VM1**. When VM1 appears in the search results select it.
2. Copy the private and public IP addresses of VM1 and the select the **Connect** button.
3. After selecting the Connect button, click on **Download RDP file**.
4. If prompted, select **Connect**. Enter the user name and password you specified when creating the VM. You may need to select **More choices**, then **Use a different account**, to specify the credentials you entered when you created the VM.
5. Select **OK**.
6. Click **No** on the Networks blade.
7. From PowerShell, enter `ping vm2`. Ping fails, why is that? **Three reasons: 1) Each virtual network is isolated from the other virtual networks, (2) ICMP is not allowed to pass through the Windows firewall by default, and (3) there is no name resolution established.**
8. To allow VM1 to ping other VMs in a later step, enter the following command from PowerShell, which allows ICMP inbound through the Windows firewall:

    `New-NetFirewallRule –DisplayName “Allow ICMPv4-In” –Protocol ICMPv4`

9. Repeat these steps (connect to the VM and issue the PowerShell command) for VM2 and VM3.

## Exercise 6 - Connect virtual networks with virtual network peering using the Azure portal

You can connect virtual networks to each other with virtual network peering. These virtual networks can be in the same region or different regions (also known as Global VNet peering). Once virtual networks are peered, resources in both virtual networks are able to communicate with each other, with the same latency and bandwidth as if the resources were in the same virtual network.

### Peer vNet1 to vNet2

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

## Exercise 7 -  Test connectivity

1. Obtain the public and private IP address of VM2.
2. Open your RDP session to VM1.
3. From PowerShell ping VM2 by Public IP address. Does PING succeed? Why or why not?
4. From PowerShell ping VM2 by Private IP address. Does PING succeed? Why or why not?

Let's examine our network topology now that we have peering enabled.

1. Click on **Network Watcher** from the left hand pane of the Azure Portal.
2. Under  **Monitoring** choose **Topology**.
3. Under **Resource Group** select **VNets**.  In a moment a conceptual network diagram should be generated showing all your vNets and subnets including the new peerings between vNet1 and vNet2.

## Challenge - PING the Public IP address

If you wanted to ping the VM by public IP address, what do you think you would have to do?

## Clean out your VMs

If you are going to complete the hub and spoke challenge keep these resources.  Otherwise, given the limited capacity of sponsored Azure please delete the **NetVMs** Resource Groups before moving onto the next lab.
