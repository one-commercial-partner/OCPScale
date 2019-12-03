# Hub and Spoke Networking Challenge

Now that you have vNet1 and vNet2 peered, resources in each virtual network and subnet can communicate with each other.  A common networking architecture is a hub-spoke network topology where the hub is a single virtual network (VNet) in Azure that acts as a central point of connectivity to your virtual networks (VNets). The spokes are VNets that peer with the hub, and can be used to isolate workloads.

Your challenge is to configure vNet2 as the hub in a hub and spoke topology.  vNet1 and vNet3 are not directly connected together.  vNet1 talks to vNet3 via vNet2 and vNet3 talks to vNet1 via vNet2.

Attempt to configure this on your own, otherwise follow these instructions.  You should configure spokes to use the hub VNet gateway to communicate with remote networks. To allow gateway traffic to flow from spoke to hub, and connect to remote networks, you must:

* Configure the VNet peering connection in the hub to allow gateway transit.
* Configure the VNet peering connection in each spoke to use remote gateways.
* Configure all VNet peering connections to allow forwarded traffic.  

Reference [Hub-spoke network topology in Azure](<https://docs.microsoft.com/en-us/azure/architecture/reference-architectures/hybrid-networking/hub-spoke>) for guidance.

## Exercise 1 - Create the third VM

We are going to create the third VM using PowerShell within Cloud Shell.  

1. Click on the Cloud Shell icon on the taskbar: **>_**
2. Select **PowerShell**.
3. If you are prompted, select **Create Storage**.
4. Enter the following to set the username and password needed for the administrator account on the VM :
    `$cred = Get-Credential`
5. Create the VM (note to use the correct region):
    `New-AzVm
    -ResourceGroupName "NetVMs"
    -Name "VM3"
    -Location "REGION"
    -VirtualNetworkName "vNet3"
    -SubnetName "Subnet3"
    -SecurityGroupName "VM3-nsg"
    -PublicIpAddressName "VM3-ip"
    -Credential $cred
    -size Standard_D2_v2`
