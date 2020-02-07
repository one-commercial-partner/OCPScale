# AZ-300: Microsoft Azure Architect Technologies Study Group
## Implement workloads and security (25-30%)

### Migrate servers to Azure
- migrate servers using Azure Migrate
  - [About Azure Migrate](https://docs.microsoft.com/en-us/azure/migrate/migrate-services-overview)
  - [Migrate Hyper-V VMs to Azure](https://docs.microsoft.com/en-us/azure/migrate/tutorial-migrate-hyper-v)
  
### Configure serverless computing
- create and manage objects
- manage a Logic App resource
  - [Overview - What is Azure Logic Apps?](https://docs.microsoft.com/en-us/azure/logic-apps/logic-apps-overview)
  - [Quickstart: Create your first workflow by using Azure Logic Apps - Azure portal](https://docs.microsoft.com/en-us/azure/logic-apps/quickstart-create-first-logic-app-workflow)
  - [Manage logic apps with Visual Studio](https://docs.microsoft.com/en-us/azure/logic-apps/manage-logic-apps-with-visual-studio)
- manage Azure Function app settings
  - [An introduction to Azure Functions](https://docs.microsoft.com/en-us/azure/azure-functions/functions-overview)
  - [Create your first function using Visual Studio](https://docs.microsoft.com/en-us/azure/azure-functions/functions-create-your-first-function-visual-studio)
  - [Manage your function app](https://docs.microsoft.com/en-us/azure/azure-functions/functions-how-to-use-azure-function-app-settings)
- manage Event Grid
  - [What is Azure Event Grid?](https://docs.microsoft.com/en-us/azure/event-grid/overview)
  - [Choose between Azure messaging services - Event Grid, Event Hubs, and Service Bus](https://docs.microsoft.com/en-us/azure/event-grid/compare-messaging-services)
  - [Quickstart: Route custom events to web endpoint with Azure CLI and Event Grid](https://docs.microsoft.com/en-us/azure/event-grid/custom-event-quickstart)
- manage Service Bus
  - [What is Azure Service Bus?](https://docs.microsoft.com/en-us/azure/service-bus-messaging/service-bus-messaging-overview)
  - [Quickstart: Use Azure portal to create a Service Bus queue](https://docs.microsoft.com/en-us/azure/service-bus-messaging/service-bus-quickstart-portal)
  - [Get started with Service Bus queues](https://docs.microsoft.com/en-us/azure/service-bus-messaging/service-bus-dotnet-get-started-with-queues)

### Implement application load balancing
- configure application gateway
  - [What is Azure Application Gateway?](https://docs.microsoft.com/en-us/azure/application-gateway/overview)
  - [Quickstart: Direct web traffic with Azure Application Gateway - Azure portal](https://docs.microsoft.com/en-us/azure/application-gateway/quick-create-portal)
- configure Azure Front Door service
  - [What is Azure Front Door Service?](https://docs.microsoft.com/en-us/azure/frontdoor/front-door-overview)
  - [Quickstart: Create a Front Door for a highly available global web application](https://docs.microsoft.com/en-us/azure/frontdoor/quickstart-create-front-door)
- configure Azure Traffic Manager
  - [What is Traffic Manager?](https://docs.microsoft.com/en-us/azure/traffic-manager/traffic-manager-overview)
  - [Quickstart: Create a Traffic Manager profile using the Azure portal](https://docs.microsoft.com/en-us/azure/traffic-manager/quickstart-create-traffic-manager-profile)

### Integrate on premises network with Azure virtual network
- create and configure Azure VPN Gateway
  - [Tutorial: Create and manage a VPN gateway using PowerShell](https://docs.microsoft.com/en-us/azure/vpn-gateway/vpn-gateway-tutorial-create-gateway-powershell)
- create and configure site to site VPN
  - [Create a Site-to-Site connection in the Azure portal](https://docs.microsoft.com/en-us/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal)
  - [Tutorial: Create and manage S2S VPN connections using PowerShell](https://docs.microsoft.com/en-us/azure/vpn-gateway/vpn-gateway-tutorial-vpnconnection-powershell)
- configure ExpressRoute
  - [Tutorial: Create and modify an ExpressRoute circuit](https://docs.microsoft.com/en-us/azure/expressroute/expressroute-howto-circuit-portal-resource-manager)
  - [Create and modify peering for an ExpressRoute circuit](https://docs.microsoft.com/en-us/azure/expressroute/expressroute-howto-routing-portal-resource-manager)
- configure Virtual WAN
  - [About Azure Virtual WAN](https://docs.microsoft.com/en-us/azure/virtual-wan/virtual-wan-about)
- verify on premises connectivity
  - [Verify a VPN Gateway connection](https://docs.microsoft.com/en-us/azure/vpn-gateway/vpn-gateway-verify-connection-resource-manager)
  - [Verifying ExpressRoute connectivity](https://docs.microsoft.com/en-us/azure/expressroute/expressroute-troubleshooting-expressroute-overview)
- troubleshoot on premises connectivity with Azure
  - [Troubleshoot VPN Gateway](https://docs.microsoft.com/en-us/azure/vpn-gateway/vpn-gateway-troubleshoot)

### Implement multi factor authentication
- configure user accounts for MFA
  - [How to require two-step verification for a user](https://docs.microsoft.com/en-us/azure/active-directory/authentication/howto-mfa-userstates)
- configure fraud alerts
  - [Configure Azure Multi-Factor Authentication settings: Fraud alert](https://docs.microsoft.com/en-us/azure/active-directory/authentication/howto-mfa-mfasettings#fraud-alert)
- configure bypass options
  - [Configure Azure Multi-Factor Authentication settings: One-time bypass](https://docs.microsoft.com/en-us/azure/active-directory/authentication/howto-mfa-mfasettings#one-time-bypass)
- configure trusted IPs
  - [Configure Azure Multi-Factor Authentication settings: Trusted IPs](https://docs.microsoft.com/en-us/azure/active-directory/authentication/howto-mfa-mfasettings#trusted-ips)
- configure verification methods
  - [Configure Azure Multi-Factor Authentication settings: Verification methods](https://docs.microsoft.com/en-us/azure/active-directory/authentication/howto-mfa-mfasettings#verification-methods)

### Manage role-based access control
- create a custom role
  - [Tutorial: Create a custom role for Azure resources using Azure PowerShell](https://docs.microsoft.com/en-us/azure/role-based-access-control/tutorial-custom-role-powershell)
- configure access to Azure resources by assigning roles
  - [Custom roles for Azure resources](https://docs.microsoft.com/en-us/azure/role-based-access-control/custom-roles)
- configure management access to Azure
  - [Manage access to Azure resources with Azure AD Privileged Identity Management](https://docs.microsoft.com/en-us/azure/role-based-access-control/pim-azure-resource)
- troubleshoot RBAC
  - [Troubleshoot RBAC for Azure resources](https://docs.microsoft.com/en-us/azure/role-based-access-control/troubleshooting)
- implement Azure Policies
  - [What is Azure Policy?](https://docs.microsoft.com/en-us/azure/governance/policy/overview)
  - [Quickstart: Create a policy assignment to identify non-compliant resources](https://docs.microsoft.com/en-us/azure/governance/policy/assign-policy-portal)
- assign RBAC Roles
  - [Add or remove role assignments using Azure RBAC and the Azure portal](https://docs.microsoft.com/en-us/azure/role-based-access-control/role-assignments-portal)

[Back](index.md)