# Deploy and Configure Infrastructure (25-30%)

## Analyze resource utilization and consumption

| Topic | Link |
| --- | --- |
| Configure diagnostic settings on resources | [Collect and consume log data from your Azure resources](https://docs.microsoft.com/en-us/azure/azure-monitor/platform/diagnostic-logs-overview) |
| Create baseline for resources | [Azure Monitor Overview](https://docs.microsoft.com/en-us/azure/azure-monitor/overview) |
| Create and rest alerts | [Create, view, and manage metric alerts using Azure Monitor](https://docs.microsoft.com/en-us/azure/azure-monitor/platform/alerts-metric) |
|   | [Introduction to Azure Advisor](https://docs.microsoft.com/en-us/azure/advisor/advisor-overview) |
| Analyze alerts across subscription | [Overview of Alerts in Microsoft Azure](https://docs.microsoft.com/en-us/azure/azure-monitor/platform/alerts-overview) |
|   | [Exciting advances in Azure Alerts – From better alert management to Smart Groups](https://azure.microsoft.com/en-us/blog/exciting-advances-in-azure-alerts-from-better-alert-management-to-smart-groups/) |
| Analyze metrics across subscription | [Collect Azure Activity Logs into Log Analytics across subscription](https://docs.microsoft.com/en-us/azure/azure-monitor/platform/collect-activity-logs-subscriptions) |
|   | [Understand how metric alerts work in Azure Monitor](https://docs.microsoft.com/en-us/azure/azure-monitor/platform/alerts-metric-overview) |
| Create action groups | [Create and manage action groups in the Azure portal](https://docs.microsoft.com/en-us/azure/azure-monitor/platform/action-groups) |
| Monitor for unused resources | [Prevent unexpected charges with Azure billing and cost management](https://docs.microsoft.com/en-us/azure/billing/billing-getting-started) |
| Monitor spend | [What is Azure Cost Management?](https://docs.microsoft.com/en-us/azure/cost-management/overview-cost-mgt) |
|   | [Pricing Calculator](https://azure.microsoft.com/en-us/pricing/calculator/) |
| Report on spend | [Azure Monitor Metrics Explorer](https://docs.microsoft.com/en-us/azure/azure-monitor/platform/metrics-charts) |
| Utilize Log Search query functions | [Get Started with queries in Log Analytics](https://docs.microsoft.com/en-us/azure/azure-monitor/log-query/get-started-queries) |
| View alerts in Log Analytics | [Overview of Alerts in Microsoft Azure](https://docs.microsoft.com/en-us/azure/azure-monitor/platform/alerts-overview) |

## Create and configure storage accounts

| Topic | Link |
| --- | --- |
| Configure network access to the storage account | [Configure Azure Storage firewalls and virtual networks](https://docs.microsoft.com/en-us/azure/storage/common/storage-network-security) |
| Create and configure storage account | [Create a storage account](https://docs.microsoft.com/en-us/azure/storage/common/storage-quickstart-create-account?tabs=azure-portal) |
| Generate shared access signature | [Using shared access signatures (SAS)](https://docs.microsoft.com/en-us/azure/storage/common/storage-dotnet-shared-access-signature-part-1) |
|   | [Shared Access Signatures, Part 2: Create and use a SAS with Blob storage](https://docs.microsoft.com/en-us/azure/storage/blobs/storage-dotnet-shared-access-signature-part-2) |
| Install and use Azure Storage Explorer | [Azure Storage Explorer](https://azure.microsoft.com/en-us/features/storage-explorer/) |
|   | [Get started with Storage Explorer](https://docs.microsoft.com/en-us/azure/vs-azure-tools-storage-manage-with-storage-explorer?toc=%2Fazure%2Fstorage%2Fqueues%2Ftoc.json&amp;tabs=windows) |
|   | [Connect storage explorer to an Azure Stack subscription or a storage account](https://docs.microsoft.com/en-us/azure/azure-stack/user/azure-stack-storage-connect-se) |
| Manage access keys | [Manage storage account settings in the Azure portal](https://docs.microsoft.com/en-us/azure/storage/common/storage-account-manage) |
| Monitor activity log by using Log Analytics | [Storage Analytics](https://docs.microsoft.com/en-us/azure/storage/common/storage-analytics) |
| Implement Azure storage replication | [Azure Storage replication](https://docs.microsoft.com/en-us/azure/storage/common/storage-redundancy) |

## Create and configure a Virtual Machine (VM) for Windows and Linux

| Topic | Link |
| --- | --- |
| Configure high availability | [Manage the availability of Windows virtual machines in Azure](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/manage-availability)[Azure and Linux](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/overview) |
| Configure monitoring, networking, storage, and virtual machine size | [How to monitor virtual machines in Azure](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/monitor) |
|   | [About disks storage for Azure Windows VMs](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/about-disks-and-vhds) |
|   | [Virtual networks and virtual machines in Azure](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/network-overview) |
| Deploy and configure scale sets | [What are virtual machine scale sets?](https://docs.microsoft.com/en-us/azure/virtual-machine-scale-sets/overview?toc=%2Fazure%2Fvirtual-machines%2Fwindows%2Ftoc.json) |

## Automate deployment of Virtual Machines (VMs)

_May include but not limited to:_ Modify Azure Resource Manager (ARM) template; configure location of new VMs; configure VHD template; deploy from template; save a deployment as an ARM template; deploy Windows and Linux VMs

| Topic | Link |
| --- | --- |
| Modify Azure Resource Manager (ARM) template | [Understand the structure and syntax of Azure Resource Manager templates](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-authoring-templates) |
| configure location of new VMs | [Create Azure Resource Manager template](https://docs.microsoft.com/en-us/azure/azure-resource-manager/how-to-create-template) |
| configure VHD template | [Tutorial - Manage Azure disks with Azure PowerShell](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/tutorial-manage-data-disk) |
| deploy from template | [Deploy resources with Resource Manager templates and Azure portal](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-template-deploy-portal) |
|   | [Deploy the template](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-manager-quickstart-create-templates-use-visual-studio-code#deploy-the-template) |
|   | [Create a Windows virtual machine from a Resource Manager template](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/ps-template) |
| save a deployment as an ARM template | [https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-manager-export-template](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-manager-export-template) |
| deploy Windows and Linux VMs | [Deploy an Azure Virtual Machine using C# and a Resource Manager template](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/csharp-template) |
|   | [How to create a Linux virtual machine with Azure Resource Manager templates](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/create-ssh-secured-vm-from-template) |

## Create connectivity between virtual networks

_May include but not limited to:_ Create and configure VNET peering; create and configure VNET to VNET; verify virtual network connectivity; create virtual network gateway

| Topic | Link |
| --- | --- |
| Create and configure VNET peering | [https://docs.microsoft.com/en-us/azure/virtual-network/virtual-network-peering-overview](https://docs.microsoft.com/en-us/azure/virtual-network/virtual-network-peering-overview) |
|   | [https://docs.microsoft.com/en-us/azure/virtual-network/virtual-network-manage-peering](https://docs.microsoft.com/en-us/azure/virtual-network/virtual-network-manage-peering) |
| create and configure VNET to VNET | [https://docs.microsoft.com/en-us/azure/vpn-gateway/vpn-gateway-connect-different-deployment-models-portal?toc=%2fazure%2fvirtual-network%2ftoc.json](https://docs.microsoft.com/en-us/azure/vpn-gateway/vpn-gateway-connect-different-deployment-models-portal?toc=%2fazure%2fvirtual-network%2ftoc.json) |
|  verify virtual network connectivity | [https://docs.microsoft.com/en-us/azure/vpn-gateway/vpn-gateway-connect-different-deployment-models-portal?toc=%2fazure%2fvirtual-network%2ftoc.json#verify](https://docs.microsoft.com/en-us/azure/vpn-gateway/vpn-gateway-connect-different-deployment-models-portal?toc=%2fazure%2fvirtual-network%2ftoc.json#verify) |
| create virtual network gateway | [https://docs.microsoft.com/en-us/azure/virtual-network/virtual-network-peering-overview#gateways-and-on-premises-connectivity](https://docs.microsoft.com/en-us/azure/virtual-network/virtual-network-peering-overview#gateways-and-on-premises-connectivity) |

## Implement and manage virtual networking

_May include but not limited to:_ Configure private and public IP addresses, network routes, network interface, subnets, and virtual network

| Topic | Link |
| --- | --- |
| Configure private and public IP addresses, network routes, network interface, subnets, and virtual network | [https://docs.microsoft.com/en-us/azure/virtual-network/virtual-network-ip-addresses-overview-arm](https://docs.microsoft.com/en-us/azure/virtual-network/virtual-network-ip-addresses-overview-arm) |
|   | [https://docs.microsoft.com/en-us/azure/virtual-network/virtual-network-deploy-static-pip-arm-portal](https://docs.microsoft.com/en-us/azure/virtual-network/virtual-network-deploy-static-pip-arm-portal) |
|   | [https://docs.microsoft.com/en-us/azure/virtual-network/virtual-network-multiple-ip-addresses-powershell](https://docs.microsoft.com/en-us/azure/virtual-network/virtual-network-multiple-ip-addresses-powershell) |
|   | [https://docs.microsoft.com/en-us/azure/virtual-machines/windows/multiple-nics?toc=%2fazure%2fvirtual-network%2ftoc.json](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/multiple-nics?toc=%2fazure%2fvirtual-network%2ftoc.json) |

## Manage Azure Active Directory (AD)



| Topic | Link |
| --- | --- |
| Add custom domains | [https://docs.microsoft.com/en-us/azure/active-directory/fundamentals/add-custom-domain](https://docs.microsoft.com/en-us/azure/active-directory/fundamentals/add-custom-domain) |
| configure Azure AD Identity Protection, Azure AD Join, and Enterprise State Roaming | [https://docs.microsoft.com/en-us/azure/active-directory/identity-protection/overview](https://docs.microsoft.com/en-us/azure/active-directory/identity-protection/overview) |
|   | [https://docs.microsoft.com/en-us/azure/active-directory/identity-protection/enable](https://docs.microsoft.com/en-us/azure/active-directory/identity-protection/enable) |
|   | [https://docs.microsoft.com/en-us/azure/active-directory/devices/azureadjoin-plan](https://docs.microsoft.com/en-us/azure/active-directory/devices/azureadjoin-plan) |
|   | [https://docs.microsoft.com/en-us/azure/active-directory/devices/enterprise-state-roaming-enable](https://docs.microsoft.com/en-us/azure/active-directory/devices/enterprise-state-roaming-enable) |
| configure self-service password reset | [https://docs.microsoft.com/en-us/azure/active-directory/authentication/tutorial-sspr-pilot](https://docs.microsoft.com/en-us/azure/active-directory/authentication/tutorial-sspr-pilot) |
|   | [https://docs.microsoft.com/en-us/azure/active-directory/authentication/tutorial-enable-writeback](https://docs.microsoft.com/en-us/azure/active-directory/authentication/tutorial-enable-writeback) |
| implement conditional access policies | [https://docs.microsoft.com/en-us/azure/active-directory/conditional-access/overview](https://docs.microsoft.com/en-us/azure/active-directory/conditional-access/overview) |
|   | [https://docs.microsoft.com/en-us/azure/active-directory/conditional-access/app-based-mfa](https://docs.microsoft.com/en-us/azure/active-directory/conditional-access/app-based-mfa) |
| manage multiple directories | [https://docs.microsoft.com/en-us/azure/active-directory/hybrid/how-to-connect-fed-single-adfs-multitenant-federation](https://docs.microsoft.com/en-us/azure/active-directory/hybrid/how-to-connect-fed-single-adfs-multitenant-federation)   |
|   | [https://docs.microsoft.com/en-us/azure/active-directory/hybrid/how-to-connect-fed-management](https://docs.microsoft.com/en-us/azure/active-directory/hybrid/how-to-connect-fed-management) |
| perform an access review | [https://docs.microsoft.com/en-us/azure/active-directory/governance/perform-access-review](https://docs.microsoft.com/en-us/azure/active-directory/governance/perform-access-review) |

## Implement and manage hybrid identities

| Topic | Link |
| --- | --- |
| Install and configure Azure AD Connect | [https://docs.microsoft.com/en-us/azure/active-directory/hybrid/how-to-connect-install-roadmap](https://docs.microsoft.com/en-us/azure/active-directory/hybrid/how-to-connect-install-roadmap) |
| configure federation and single sign-on | [https://docs.microsoft.com/en-us/azure/active-directory/hybrid/how-to-connect-fed-management](https://docs.microsoft.com/en-us/azure/active-directory/hybrid/how-to-connect-fed-management) plus all Next Steps |
|   | https://docs.microsoft.com/en-us/azure/active-directory/hybrid/how-to-connect-sso-quick-start |
|  manage Azure AD Connect | [https://docs.microsoft.com/en-us/azure/active-directory/hybrid/how-to-connect-device-writeback](https://docs.microsoft.com/en-us/azure/active-directory/hybrid/how-to-connect-device-writeback) plus all Next Steps |
| manage password sync and writeback | [https://docs.microsoft.com/en-us/azure/active-directory/hybrid/whatis-phs](https://docs.microsoft.com/en-us/azure/active-directory/hybrid/whatis-phs) |
|   | [https://docs.microsoft.com/en-us/azure/active-directory/hybrid/how-to-connect-password-hash-synchronization](https://docs.microsoft.com/en-us/azure/active-directory/hybrid/how-to-connect-password-hash-synchronization) |
|   | [https://docs.microsoft.com/en-us/azure/active-directory/hybrid/how-to-connect-device-writeback](https://docs.microsoft.com/en-us/azure/active-directory/hybrid/how-to-connect-device-writeback) |
|   | [https://docs.microsoft.com/en-us/azure/active-directory/hybrid/how-to-connect-pta](https://docs.microsoft.com/en-us/azure/active-directory/hybrid/how-to-connect-pta) |