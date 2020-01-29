# AZ-300: Microsoft Azure Architect Technologies Study Group
## Deploy and configure infrastructure (40-45%)

### Analyze resource utilization and consumption
- configure diagnostic settings on resources
  - [Overview of Azure platform logs](https://docs.microsoft.com/en-us/azure/azure-monitor/platform/platform-logs-overview)
- create baseline for resources
  - [Azure Monitor overview](https://docs.microsoft.com/en-us/azure/azure-monitor/overview)
- create and test alerts
  - [Create, view, and manage metric alerts using Azure Monitor](https://docs.microsoft.com/en-us/azure/azure-monitor/platform/alerts-metric)
  - [Introduction to Azure Advisor](https://docs.microsoft.com/en-us/azure/advisor/advisor-overview)
- analyze alerts across subscription
  - [Overview of alerts in Microsoft Azure](https://docs.microsoft.com/en-us/azure/azure-monitor/platform/alerts-overview)
  - [Exciting advances in Azure Alerts – From better alert management to Smart Groups](https://azure.microsoft.com/en-us/blog/exciting-advances-in-azure-alerts-from-better-alert-management-to-smart-groups/)
- analyze metrics across subscription
  - [Collect and analyze Azure activity logs in Log Analytics workspace in Azure Monitor](https://docs.microsoft.com/en-us/azure/azure-monitor/platform/activity-log-collect)
  - [Understand how metric alerts work in Azure Monitor](https://docs.microsoft.com/en-us/azure/azure-monitor/platform/alerts-metric-overview)
- create action groups
  - [Create and manage action groups in the Azure portal](https://docs.microsoft.com/en-us/azure/azure-monitor/platform/action-groups)
- monitor for unused resources
  - [Prevent unexpected charges with Azure billing and cost management](https://docs.microsoft.com/en-us/azure/cost-management-billing/manage/getting-started)
- monitor spend
  - [What is Azure Cost Management and Billing?](https://docs.microsoft.com/en-us/azure/cost-management-billing/cost-management-billing-overview)
  - [Pricing calculator](https://azure.microsoft.com/en-us/pricing/calculator/)
- report on spend
  - [Advanced features of Azure Metrics Explorer](https://docs.microsoft.com/en-us/azure/azure-monitor/platform/metrics-charts)
- utilize Log Search query functions
  - [Get started with log queries in Azure Monitor](https://docs.microsoft.com/en-us/azure/azure-monitor/log-query/get-started-queries)
- view alerts in Azure Monitor logs
  - [Create, view, and manage log alerts using Azure Monitor](https://docs.microsoft.com/en-us/azure/azure-monitor/platform/alerts-log)
- visualize diagnostics data using Azure Monitor Workbooks
  - [Azure Monitor Workbooks](https://docs.microsoft.com/en-us/azure/azure-monitor/platform/workbooks-overview)

### Create and configure storage accounts
- configure network access to the storage account
  - [Configure Azure Storage firewalls and virtual networks](https://docs.microsoft.com/en-us/azure/storage/common/storage-network-security)
- create and configure storage account
  - [Create an Azure Storage account](https://docs.microsoft.com/en-us/azure/storage/common/storage-account-create)
- generate shared access signature
  - [Grant limited access to Azure Storage resources using shared access signatures (SAS)](https://docs.microsoft.com/en-us/azure/storage/common/storage-sas-overview)
- implement Azure AD authentication for storage
  - [Authorize access to blobs and queues using Azure Active Directory](https://docs.microsoft.com/en-us/azure/storage/common/storage-auth-aad)
- install and use Azure Storage Explorer
  - [Azure Storage Explorer](https://azure.microsoft.com/en-us/features/storage-explorer/)
  - [Get started with Storage Explorer](https://docs.microsoft.com/en-us/azure/vs-azure-tools-storage-manage-with-storage-explorer)
  - [Connect Storage Explorer to an Azure Stack Hub subscription or a storage account](https://docs.microsoft.com/en-us/azure-stack/user/azure-stack-storage-connect-se)
- manage access keys
  - [Manage storage account access keys](https://docs.microsoft.com/en-us/azure/storage/common/storage-account-keys-manage)
- monitor activity log by using Azure Monitor logs
  - [Azure Storage metrics in Azure Monitor](https://docs.microsoft.com/en-us/azure/storage/common/storage-metrics-in-azure-monitor)
- implement Azure storage replication
  - [Azure Storage redundancy](https://docs.microsoft.com/en-us/azure/storage/common/storage-redundancy)
- implement Azure storage account failover
  - [Disaster recovery and account failover](https://docs.microsoft.com/en-us/azure/storage/common/storage-disaster-recovery-guidance)

### Create and configure a VM for Windows and Linux
- configure high availability
  - [Manage the availability of Windows virtual machines in Azure](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/manage-availability)
- configure monitoring
  - [How to monitor virtual machines in Azure](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/monitor)
- configure networking
  - [Virtual networks and virtual machines in Azure](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/network-overview)
- configure storage
  - [Introduction to Azure managed disks](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/managed-disks-overview)
- configure virtual machine size
  - [Resize a Windows VM](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/resize-vm)
- implement dedicated hosts
  - [Azure Dedicated Hosts](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/dedicated-hosts)
- deploy and configure scale sets
  - [What are virtual machine scale sets?](https://docs.microsoft.com/en-us/azure/virtual-machine-scale-sets/overview)

### Automate deployment of VMs
- modify Azure Resource Manager template
  - [Understand the structure and syntax of Azure Resource Manager templates](https://docs.microsoft.com/en-us/azure/azure-resource-manager/templates/template-syntax)
- configure location of new VMs
  - [Set resource location in Resource Manager template](https://docs.microsoft.com/en-us/azure/azure-resource-manager/templates/resource-location)
- configure VHD template
  - [Create a managed image of a generalized VM in Azure](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/capture-image-resource)
- deploy from template
  - [Create a VM from a managed image](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/create-vm-generalized-managed)
- save a deployment as an Azure Resource Manager template
  - [Single and multi-resource export to a template in Azure portal](https://docs.microsoft.com/en-us/azure/azure-resource-manager/templates/export-template-portal)
- deploy Windows and Linux VMs
  - [Create a Windows virtual machine from a Resource Manager template](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/ps-template)
  - [How to create a Linux virtual machine with Azure Resource Manager templates](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/create-ssh-secured-vm-from-template)

### Create connectivity between virtual networks
- create and configure Vnet peering
  - [Virtual network peering](https://docs.microsoft.com/en-us/azure/virtual-network/virtual-network-peering-overview)
  - [Create, change, or delete a virtual network peering](https://docs.microsoft.com/en-us/azure/virtual-network/virtual-network-manage-peering)
- create and configure Vnet to Vnet connections
  - [Configure a VNet-to-VNet VPN gateway connection using PowerShell](https://docs.microsoft.com/en-us/azure/vpn-gateway/vpn-gateway-vnet-vnet-rm-ps)
- verify virtual network connectivity
  - [Configure a VNet-to-VNet VPN gateway connection using PowerShell: How to verify a connection](https://docs.microsoft.com/en-us/azure/vpn-gateway/vpn-gateway-vnet-vnet-rm-ps#verify)
- create virtual network gateway
  - [Create a route-based VPN gateway using the Azure portal](https://docs.microsoft.com/en-us/azure/vpn-gateway/create-routebased-vpn-gateway-portal)

### Implement and manage virtual networking
- configure private IP addressing
  - [IP address types and allocation methods in Azure](https://docs.microsoft.com/en-us/azure/virtual-network/virtual-network-ip-addresses-overview-arm)
  - [Configure private IP addresses for a virtual machine using the Azure portal](https://docs.microsoft.com/en-us/azure/virtual-network/virtual-networks-static-private-ip-arm-pportal)
- configure public IP addresses
  - [Create a virtual machine with a static public IP address using the Azure portal](https://docs.microsoft.com/en-us/azure/virtual-network/virtual-network-deploy-static-pip-arm-portal)
  - [Assign multiple IP addresses to virtual machines using PowerShell](https://docs.microsoft.com/en-us/azure/virtual-network/virtual-network-multiple-ip-addresses-powershell)
- create and configure network routes
  - [Virtual network traffic routing](https://docs.microsoft.com/en-us/azure/virtual-network/virtual-networks-udr-overview)
- create and configure network interface
  - [Create and manage a Windows virtual machine that has multiple NICs](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/multiple-nics)
- create and configure subnets
  - [Add, change, or delete a virtual network subnet](https://docs.microsoft.com/en-us/azure/virtual-network/virtual-network-manage-subnet)
- create and configure virtual network
  - [Create, change, or delete a virtual network](https://docs.microsoft.com/en-us/azure/virtual-network/manage-virtual-network)
- create and configure Network Security Groups and Application Security Groups
  - [Security groups](https://docs.microsoft.com/en-us/azure/virtual-network/security-overview)
  - [Create, change, or delete a network security group](https://docs.microsoft.com/en-us/azure/virtual-network/manage-network-security-group)

### Manage Azure Active Directory
- add custom domains
  - [Add your custom domain name using the Azure Active Directory portal](https://docs.microsoft.com/en-us/azure/active-directory/fundamentals/add-custom-domain)
- configure Azure AD Identity Protection
  - [What is Azure Active Directory Identity Protection?](https://docs.microsoft.com/en-us/azure/active-directory/identity-protection/overview-identity-protection)
- configure Azure AD Join
  - [How to: Plan your Azure AD join implementation](https://docs.microsoft.com/en-us/azure/active-directory/devices/azureadjoin-plan)
- configure self-service password reset
  - [Tutorial: Complete an Azure AD self-service password reset pilot roll out](https://docs.microsoft.com/en-us/azure/active-directory/authentication/tutorial-sspr-pilot)
- implement conditional access policies
  - [What is Conditional Access?](https://docs.microsoft.com/en-us/azure/active-directory/conditional-access/overview)
  - [Quickstart: Require MFA for specific apps with Azure Active Directory Conditional Access](https://docs.microsoft.com/en-us/azure/active-directory/conditional-access/app-based-mfa)
- manage multiple directories
  - [Understand how multiple Azure Active Directory tenants interact](https://docs.microsoft.com/en-us/azure/active-directory/users-groups-roles/licensing-directory-independence)
- perform an access review
  - [Review access to groups and applications in Azure AD access reviews](https://docs.microsoft.com/en-us/azure/active-directory/governance/perform-access-review)

### Implement and manage hybrid identities
- install and configure Azure AD Connect
  - [Azure AD Connect and Azure AD Connect Health installation roadmap](https://docs.microsoft.com/en-us/azure/active-directory/hybrid/how-to-connect-install-roadmap)
- configure federation
  - [Manage and customize Active Directory Federation Services by using Azure AD Connect](https://docs.microsoft.com/en-us/azure/active-directory/hybrid/how-to-connect-fed-management)
- configure single sign-on
  - [Azure Active Directory Seamless Single Sign-On: Quick start](https://docs.microsoft.com/en-us/azure/active-directory/hybrid/how-to-connect-sso-quick-start)
- manage and troubleshoot Azure AD Connect
  - [Troubleshoot Azure AD connectivity with the ADConnectivityTool PowerShell module](https://docs.microsoft.com/en-us/azure/active-directory/hybrid/how-to-connect-adconnectivitytools)
- troubleshoot password sync and writeback
  - [What is password hash synchronization with Azure AD?](https://docs.microsoft.com/en-us/azure/active-directory/hybrid/whatis-phs)
  - [Implement password hash synchronization with Azure AD Connect sync](https://docs.microsoft.com/en-us/azure/active-directory/hybrid/how-to-connect-password-hash-synchronization)
  - [Azure AD Connect: Enabling device writeback](https://docs.microsoft.com/en-us/azure/active-directory/hybrid/how-to-connect-device-writeback)

### Implement solutions that use virtual machines (VM)
- provision VMs
  - [Quickstart: Create a Windows virtual machine in the Azure portal](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/quick-create-portal)
  - [Quickstart: Create a Linux virtual machine in the Azure portal](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/quick-create-portal)
- create Azure Resource Manager templates
  - [Quickstart: Create and deploy Azure Resource Manager templates by using the Azure portal](https://docs.microsoft.com/en-us/azure/azure-resource-manager/templates/quickstart-create-templates-use-the-portal)
- configure Azure Disk Encryption for VMs
  - [Azure Disk Encryption for Windows VMs](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/disk-encryption-overview)
  - [Azure Disk Encryption for Linux VMs](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/disk-encryption-overview)
- implement Azure Backup for VMs
  - [Tutorial: Back up and restore files for Windows virtual machines in Azure](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/tutorial-backup-vms)
  - [Tutorial: Back up and restore files for Linux virtual machines in Azure](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/tutorial-backup-vms)