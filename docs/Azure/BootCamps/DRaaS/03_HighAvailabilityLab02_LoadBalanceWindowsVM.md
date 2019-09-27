# Load Balance Virtual Machines 
Load balancing provides a higher level of availability by spreading incoming requests across multiple virtual machines. In this tutorial, you learn about the different components of the Azure load balancer that distribute traffic and provide high availability.

In this lab you will:
* Create an Azure load balancer
* Create a load balancer health probe
* Create load balancer traffic rules
* Use the Custom Script Extension to create a basic IIS site
* Create virtual machines and attach to a load balancer
* View a load balancer in action
* Add and remove VMs from a load balancer

## Task 1 - Create a Resource Group
1. Log on to the Azure portal.
2. Launch an Azure Cloud Shell, which is the **>_** symbol in the Azure Portal.
3. Choose **PowerShell**, then **Create storage**.  The Azure Cloud shell will build and launch leaving you at the `PS Azure:\>` prompt.
4. Create a resource group by executing the following command:

    `New-AzResourceGroup -Name LBRG -Location EastUS`

## Task 2 - Create a public IP address
1. To access your app on the Internet, you need a public IP address for the load balancer.  Enter the following:

    `$publicIP = New-AzPublicIpAddress -ResourceGroupName "LBRG"   -Location "EastUS" -AllocationMethod "Static" -Name "PublicIP" `

## Task 3 - Create a load balancer
1. Create a frontend IP pool named **myFrontEndPool** and attach it  the **myPublicIP** address:

    `$frontendIP = New-AzLoadBalancerFrontendIpConfig -Name "FrontEndPool" -PublicIpAddress $publicIP`

2. Create a backend address pool:

    `$backendPool = New-AzLoadBalancerBackendAddressPoolConfig -Name "BackEndPool"`

3. Create the load balancer itself:

    `$lb = New-AzLoadBalancer -ResourceGroupName "LBRG" -Name "LoadBalancer" -Location "EastUS" -FrontendIpConfiguration $frontendIP -BackendAddressPool $backendPool`

## Task 4 - Create a health probe
To allow the load balancer to monitor the status of your app, you use a health probe. The health probe dynamically adds or removes VMs from the load balancer rotation based on their response to health checks. By default, a VM is removed from the load balancer distribution after two consecutive failures at 15-second intervals. You create a health probe based on a protocol or a specific health check page for your app.

The following example creates a TCP probe. You can also create custom HTTP probes for more fine grained health checks. When using a custom HTTP probe, you must create the health check page, such as healthcheck.aspx. The probe must return an HTTP 200 OK response for the load balancer to keep the host in rotation.

1. Create a health probe by entering:

    `Add-AzLoadBalancerProbeConfig -Name "HealthProbe"   -LoadBalancer $lb -Protocol tcp -Port 80 -IntervalInSeconds 15 
  -ProbeCount 2`

  2. Apply the health probe by entering:
    `Set-AzLoadBalancer -LoadBalancer $lb`

## Task 5 - Create a load balancer rule
A load balancer rule is used to define how traffic is distributed to the VMs. You define the front-end IP configuration for the incoming traffic and the back-end IP pool to receive the traffic, along with the required source and destination port. To make sure only healthy VMs receive traffic, you also define the health probe to use.

1. Create a load balancer rule by entering the variable:

    `$probe = Get-AzLoadBalancerProbeConfig -LoadBalancer $lb -Name "HealthProbe"`

2. Create the actual rule:

    `Add-AzLoadBalancerRuleConfig -Name "LoadBalancerRule"   -LoadBalancer $lb -FrontendIpConfiguration $lb.FrontendIpConfigurations[0] -BackendAddressPool $lb.BackendAddressPools[0] -Protocol Tcp   -FrontendPort 80 -BackendPort 80 -Probe $probe`

3. Update the load balancer:

    `Set-AzLoadBalancer -LoadBalancer $lb`

## Task 6 - Configure virtual network
Before you deploy some VMs and can test your balancer, create the supporting virtual network resources.

1. Create the subnet:

    `$subnetConfig = New-AzVirtualNetworkSubnetConfig -Name "Subnet" -AddressPrefix 192.168.1.0/24`

2. Create the virtual network:

    `$vnet = New-AzVirtualNetwork -ResourceGroupName "LBRG"   -Location "EastUS" -Name "Vnet" -AddressPrefix 192.168.0.0/16 
  -Subnet $subnetConfig`

3. Creates three virtual NICs, one virtual NIC for each VM you create for your app:

    `for ($i=1; $i -le 3; $i++)
{
   New-AzNetworkInterface -ResourceGroupName "LBRG" -Name VM$i      -Location "EastUS" -Subnet $vnet.Subnets[0] 
     -LoadBalancerBackendAddressPool $lb.BackendAddressPools[0]
}`

## Task 7 - Create virtual machines
Time to built out  your VMs:
1. Create an availability set:

    `$availabilitySet = New-AzAvailabilitySet 
  -ResourceGroupName "LBRG" -Name "AVSet" -Location "EastUS" 
  -Sku aligned -PlatformFaultDomainCount 2 
  -PlatformUpdateDomainCount 2`

2. Set an administrator username and password for the VMs:

    `$cred = Get-Credential`


3. Create the VMs:

    `for ($i=1; $i -le 3; $i++)
{
    New-AzVm 
        -ResourceGroupName "LBRG" -Name "VM$i" -Location "East US" 
        -VirtualNetworkName "Vnet" -SubnetName "Subnet" 
        -SecurityGroupName "NSG" -OpenPorts 80 
        -AvailabilitySetName "AVSet" 
        -Credential $cred -AsJob
}`

    The `-AsJob` parameter creates the VM as a background task, so the PowerShell prompts return to you. Monitor the status of build process by clicking on **Virtual Machines** in the Azure portal.

## Task 8 - Install IIS with Custom Script Extension
We are going to use a Custom Script Extension the runs powershell `Add-WindowsFeature Web-Server` to install the IIS webserver and then updates the Default.htm page to show the hostname of the VM.  

**Do not start this step until all three VMs are built and running.**
1. Enter this command from the Azure CLoud Shell:

    `for ($i=1; $i -le 3; $i++)
{
   Set-AzVMExtension 
     -ResourceGroupName "LBRG" -ExtensionName "IIS" 
     -VMName VM$i 
     -Publisher Microsoft.Compute 
     -ExtensionType CustomScriptExtension 
     -TypeHandlerVersion 1.8 
     -SettingString '{"commandToExecute":"powershell Add-WindowsFeature Web-Server; powershell Add-Content -Path \"C:\\inetpub\\wwwroot\\Default.htm\" -Value $($env:computername)"}' 
     -Location EastUS
}`

    When you hit enter, the prompt in the Cloud Shell won't immediately return.  The process is running in the foreground and has control of the shell as we did not use the `-AsJob` parameter.  This task will take upwards of 10 minutes to run, and all three `IsSuccessStatusCode` should be `True`.

## Task 8 - Test load balancer
Obtain the public IP address of your load balancer with `Get-AzPublicIPAddress`. 

1. Enter this command into Azure Cloud Shell:

    `Get-AzPublicIPAddress -ResourceGroupName "LBRG" -Name "PublicIP" | select IpAddress`

2. To see the load balancer distribute traffic across all three VMs running your app, open a web browser and surf to your public IP address and intermittantly force-refresh your web browser.
3. Stop **VM1** from the Azure Portal and force-refresh your web browser. To see the load balancer distribute traffic across the remaining two VMs running your app you can force-refresh your web browser. You can now perform maintenance on the VM, such as installing OS updates or performing a VM reboot.
4. From the Azure Portal check the status of the Back End Pool by navigating to **Resource groups > LBRG > LoadBalancer - Backend pools** and click on  **BackEndPool**. Notice the status of VM1.
5. Stop VM2 from the Azure Portal and force-refresh your web browse to see the load balancer distribute traffic across the remaining VM.

Once you are done you may want to clean up the resources we created.  Use the following PowerShell command to delete the resources we created in this lab:

`Remove-AzResourceGroup -Name "LBRG"`



[Back](../)