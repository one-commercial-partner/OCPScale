# Lab 1 - Virtual Networks

## Lab Summary

In this lab you are going to create multiple virtual networks each with it's own virtual machine and subnet and then test connectivity across subnets and vnets.

## Exercise 1 -  Create virtual networks

### Task 1 - Create vNet1

1. Log in to the Azure portal at <https://portal.azure.com> and click on **Microsoft Azure**, and then **+Create a resource**.
2. Select **Networking**, and then select **Virtual network**.
3. Enter the following information on the **Basics** tab and then click on **Next: IP Addresses >**
    * Resource Group: *Create New* **VNets**
    * Name: **vNet1**
4. Enter the following information on the **IP Addresses** tab and then click on **Next: IP Addresses >**
    * Click the garbage can to delete the existing address space of 10.0.0.0/16.
    * Enter a new IPv4 Address Space: **10.101.0.0/16**
    * Click on **+ Add subnet** and enter the Subnet Name of **subnet1** and a subnet address range of **10.101.1.0/24** and then click **Add**.
5. Click on **Review + Create**, and after validation, click **Create**.

### Task 2 - Create vNet2

Repeat the steps above for vNet2:

* Name: **vNet2**
* Address Space: **10.102.0.0/16**
* Resource Group: **VNets**
* Subnet Name: **subnet2**
* Subnet address range: **10.102.2.0/24**

## Exercise 2 - Provision a virtual machine via the portal

1. Return to the Azure portal and click the **+Create a Resource** button.
2. Select **Compute** then select **Virtual machine**.
3. On the Basics tab complete the following:
    * Resource Group:   **VNets**
    * Virtual machine name: **VM1**
    * Region: Choose the same region as your other resources
    * Availability options: No infrastructure redundancy required
    * Image: Windows Server 2016 Datacenter
    * Size: Choose **DS2_v2** (or something similar)
    * Username: *yourfirstname* and write it down
    * Password: `Complex.Password`
    * Confirm Password: `Complex.Password`
    * Public inbound ports: **Allow selected ports**
    * Select inbound ports: **RDP (3389)**
4. Click **Next : Disks >** and then **Next : Networking >**
5. Ensure **vNet1** is selected for the Virtual network.
6. **Review + create**.   After validation passes, click  **Create** and then monitor your deployment status for any error messages. It should take less than 10 minutes to spin up the VM.

## Exercise 3 - Provision a virtual machine via Cloud Shell

1. Click on the Cloud Shell icon on the taskbar: **>_**
2. Change the shell type to  **PowerShell** and click **Confirm** to switch to PowerShell.
3. If you are prompted, select **Create Storage**.
4. Enter the following to set the username and password needed for the administrator account on the VM.  Enter *yourfirstname* as the User and then `Complex.Password` as the password.

    `$cred = Get-Credential`
5. Create the VM (note to use the correct region):

    `New-AzVm
    -ResourceGroupName "VNets"
    -Name "VM2"
    -Location "EastUS"
    -VirtualNetworkName "vNet2"
    -SubnetName "subnet2"
    -SecurityGroupName "VM2-nsg"
    -PublicIpAddressName "VM2-ip"
    -Credential $cred
    -size Standard_DS2_v2`

## Exercise 4 - Validate Your Configuration

You now have two virtual machines each in their own virtual network and subnet. Let's validate that.

1. In the search bar of the Azure Portal enter  **Network Watcher**.
2. Under  **Monitoring** choose **Topology**.
3. Under **Resource Group** select **VNets**.  In a moment a conceptual network diagram should be generated showing all vNets and subnets.  Notice that there is no link between vNet1 or vNet2.

## Exercise 5 - Connect to a VM and test connectivity

Before you begin this section, obtain the private and public IP addresses of VM1 and VM2.

1. At the top of the Azure portal in the Search bar enter **VM1**. When VM1 appears in the search results select it.
2. Copy the private and public IP addresses of VM1 and then click on  **Connect** and then **RDP**.
3. Click on **Download RDP file**.
4. If prompted, select **Connect**. Enter the user name and password you specified when creating the VM. You may need to select **More choices**, then **Use a different account**, to specify the credentials you entered when you created the VM.
5. Select **OK**.
6. Click **No** on the Networks blade.
7. From PowerShell, enter `ping vm2`. Ping fails, why is that?

    *Three reasons: 1) Each virtual network is isolated from every other virtual networks by default (2) ICMP is not allowed to pass through the Windows firewall by default, and (3) there is no name resolution established.*

8. To allow VM1 to ping other VMs in a later step, enter the following command from PowerShell which allows ICMP inbound through the Windows firewall:

    `New-NetFirewallRule –DisplayName “Allow ICMPv4-In” –Protocol ICMPv4`

9. Repeat these steps (connect to the VM and issue the PowerShell command) for VM2.

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

    *PING by public IP address will fail as the packets need to go outside of Azure and come back in, and by default NSG's block all incoming traffic.*

4. From PowerShell ping VM2 by Private IP address. Does PING succeed? Why or why not?

    *PING by private IP address will succeed as the packets stay within the Azure network and utilize the peering you just configured.*

Let's examine our network topology now that we have peering enabled.

1. Click on **Network Watcher** from the left hand pane of the Azure Portal.
2. Under  **Monitoring** choose **Topology**.
3. Under **Resource Group** select **VNets**.  In a moment a conceptual network diagram should be generated showing the new peerings between vNet1 and vNet2.

## Challenge - PING the Public IP address

If you wanted to ping the VM by public IP address, what do you think you would have to do?
