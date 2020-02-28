# Configure and manage virtual networks (30-35%)

*Note than many topics below can be performed in the Azure portal,  Powershell, and CLI.  Ensure you become familiar with all three methods for the exam.*

## Create connectivity between virtual networks

*Note than many topics below can be performed in the Azure portal,  Powershell, and CLI.  Ensure you become familiar with all three methods for the exam.*

| Topic | Link |
| - | - |
| create and configure VNET peering|[Virtual network peering](https://docs.microsoft.com/en-us/azure/virtual-network/virtual-network-peering-overview)|
| |[Create, change, or delete a virtual network peering](https://docs.microsoft.com/en-us/azure/virtual-network/virtual-network-manage-peering)|
| |[Connect virtual networks with virtual network peering using PowerShell](https://docs.microsoft.com/en-us/azure/virtual-network/tutorial-connect-virtual-networks-powershell)|
| create and configure VNET to VNET connections|[Configure a VNet-to-VNet VPN gateway connection by using the Azure portal](https://docs.microsoft.com/en-us/azure/vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal)|
| verify virtual network connectivity|[Verify a VPN Gateway connection](https://docs.microsoft.com/en-us/azure/vpn-gateway/vpn-gateway-verify-connection-resource-manager) |
| create virtual network gateway|[Create a route-based VPN gateway using the Azure portal](https://docs.microsoft.com/en-us/azure/vpn-gateway/create-routebased-vpn-gateway-portal) |
|Experiential Learning|[Distribute your services across Azure virtual networks and integrate them by using virtual network peering](https://docs.microsoft.com/en-us/learn/modules/integrate-vnets-with-vnet-peering/)|
||[Lab: VNet Peering and Service Chaining](https://github.com/MicrosoftLearning/AZ-103-MicrosoftAzureAdministrator/blob/master/Instructions/Labs/05%20-%20VNet%20Peering%20and%20Service%20Chaining%20(az-100-04).md)|


## Implement and manage virtual networking

| Topic | Link |
| - | - |
|configure private and public IP addresses, network routes, network interface, subnets, and virtual network|[IP address types and allocation methods in Azure](https://docs.microsoft.com/en-us/azure/virtual-network/virtual-network-ip-addresses-overview-arm)|
| |[Create a virtual machine with a static public IP address using the Azure portal](https://docs.microsoft.com/en-us/azure/virtual-network/virtual-network-deploy-static-pip-arm-portal) |
| |[Assign multiple IP addresses to virtual machines using PowerShell](https://docs.microsoft.com/en-us/azure/virtual-network/virtual-network-multiple-ip-addresses-powershell) |
| |[Create and manage a Windows virtual machine that has multiple NICs](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/multiple-nics?toc=%2fazure%2fvirtual-network%2ftoc.json)|


## Configure name resolution

| Topic | Link |
| - | - |
|configure Azure DNS |[What is Azure DNS?](https://docs.microsoft.com/en-us/azure/dns/dns-overview) |
|configure custom DNS settings|[How to manage DNS Zones in the Azure portal](https://docs.microsoft.com/en-us/azure/dns/dns-operations-dnszones-portal)|
|configure private and public DNS zones|[Quickstart: Create an Azure DNS zone and record using the Azure portal](https://docs.microsoft.com/en-us/azure/dns/dns-getstarted-portal)|
| |[Manage DNS records and record sets by using the Azure portal](https://docs.microsoft.com/en-us/azure/dns/dns-operations-recordsets-portal)
| |[Quickstart: Create an Azure private DNS zone using the Azure portal](https://docs.microsoft.com/en-us/azure/dns/private-dns-getstarted-portal)|
|Experiential Learning| [Lab: Configure Azure DNS](https://github.com/MicrosoftLearning/AZ-103-MicrosoftAzureAdministrator/blob/master/Instructions/Labs/04%20-%20Configure%20Azure%20DNS%20(az-100-04b).md#lab-configure-azure-dns)


## Create and configure a Network Security Group (NSG)

| Topic | Link |
| - | - |
|create security rules|[Tutorial: Filter network traffic with a network security group using the Azure portal](https://docs.microsoft.com/en-us/azure/virtual-network/tutorial-filter-network-traffic) |
| associate NSG to a subnet or network interface|[Create, change, or delete a network security group](https://docs.microsoft.com/en-us/azure/virtual-network/manage-network-security-group) |
| evaluate effective security rules|[Diagnose a virtual machine routing problem](https://docs.microsoft.com/en-us/azure/virtual-network/diagnose-network-routing-problem)|
| implement Application Security Groups|[Application security groups](https://docs.microsoft.com/en-us/azure/virtual-network/security-overview#application-security-groups) |
| experiential learning|[Tutorial: Filter network traffic with a network security group using the Azure portal](https://docs.microsoft.com/en-us/azure/virtual-network/tutorial-filter-network-traffic)|
| |[Filter network traffic with a network security group using PowerShell](https://docs.microsoft.com/en-us/azure/virtual-network/tutorial-filter-network-traffic-powershell)|
| |[Create, change, or delete a network security group](https://docs.microsoft.com/en-us/azure/virtual-network/manage-network-security-group) |
| |[Diagnostic logging for a network security group](https://docs.microsoft.com/en-us/azure/virtual-network/virtual-network-nsg-manage-log)|
| |[Diagnose a virtual machine network traffic filter problem](https://docs.microsoft.com/en-us/azure/virtual-network/diagnose-network-traffic-filter-problem) |


## Implement Azure load balancer

| Topic | Link |
| - | - |
| |[What is Azure Load Balancer?](https://docs.microsoft.com/en-us/azure/load-balancer/load-balancer-overview)|
|configure internal load balancer| [Create an internal load balancer by using the Azure PowerShell module](https://docs.microsoft.com/en-us/azure/load-balancer/load-balancer-get-started-ilb-arm-ps) |
| configure load balancing rules|[Configure load balancing and outbound rules in Standard Load Balancer by using the Azure portal](https://docs.microsoft.com/en-us/azure/load-balancer/configure-load-balancer-outbound-portal) |
| configure public load balancer |[Lab: Load Balancer and Traffic Manager](https://github.com/MicrosoftLearning/AZ-103-MicrosoftAzureAdministrator/blob/master/Instructions/Labs/08%20-%20Load%20Balancer%20and%20Traffic%20Manager%20(az-101-03).md)|
| troubleshoot load balancing|[Troubleshoot Azure Load Balancer](https://docs.microsoft.com/en-us/azure/load-balancer/load-balancer-troubleshoot) |
|Experiential Learning| [Improve application scalability and resiliency by using Azure Load Balancer](https://docs.microsoft.com/en-us/learn/modules/improve-app-scalability-resiliency-with-load-balancer/)|


## Monitor and troubleshoot virtual networking

| Topic | Link |
| - | - |
| monitor on-premises connectivity,  troubleshoot external networking, troubleshoot virtual network connectivity|[Diagnose on-premises connectivity via VPN gateways](https://docs.microsoft.com/en-us/azure/network-watcher/network-watcher-diagnose-on-premises-connectivity)|
| use Network resource monitoring|[Tutorial: Log network traffic to and from a virtual machine using the Azure portal](https://docs.microsoft.com/en-us/azure/network-watcher/network-watcher-nsg-flow-logging-portal)|
| use Network Watcher|[What is Azure Network Watcher?](https://docs.microsoft.com/en-us/azure/network-watcher/network-watcher-monitoring-overview)|


## Integrate on premises network with Azure virtual network

| Topic | Link |
| - | - |
| create and configure Azure VPN Gateway|[Connect your on-premises network to Azure with VPN Gateway](https://docs.microsoft.com/en-us/learn/modules/connect-on-premises-network-with-vpn-gateway/) |
| create and configure site to site VPN|[Create a Site-to-Site connection in the Azure portal](https://docs.microsoft.com/en-us/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal) |
| configure Express Route| [Connect your on-premises network to the Microsoft global network by using ExpressRoute](https://docs.microsoft.com/en-us/learn/modules/connect-on-premises-network-with-expressroute/)|
| verify on premises connectivity|[Verifying ExpressRoute connectivity](https://docs.microsoft.com/en-us/azure/expressroute/expressroute-troubleshooting-expressroute-overview)|
| troubleshoot on premises connectivity with Azure|[Troubleshoot connections with Azure Network Watcher using the Azure portal](https://docs.microsoft.com/en-us/azure/network-watcher/network-watcher-connectivity-portal)|
| |[Diagnose on-premises connectivity via VPN gateways](https://docs.microsoft.com/en-us/azure/network-watcher/network-watcher-diagnose-on-premises-connectivity)|

