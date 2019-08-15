# Virtual Machine Durability Labs - Availability Sets


In this lab you learn how to increase the availability and reliability of your Virtual Machines (VMs) using Availability Sets. Availability Sets make sure the VMs you deploy on Azure are distributed across multiple, isolated hardware nodes.

An Availability Set is a logical grouping capability for isolating VM resources from each other when they're deployed. Azure makes sure that the VMs you place within an Availability Set run across multiple physical servers, compute racks, storage units, and network switches. If a hardware or software failure happens, only a subset of your VMs are impacted and your overall solution stays operational. Availability Sets are essential for building reliable cloud solutions.

You will be completing this lab in the Azure Cloud Shell.

## Task 1 - Create an availability set
1. Log on to the Azure portal.
2. Launch an Azure Cloud Shell, which is the **>_** symbol in the Azure Portal.
3. Choose **PowerShell**, then **Create storage**.  The Azure Cloud shell will build and launch leaving you at the `PS Azure:\>` prompt.
4. Create a resource group by executing the following command:

    `New-AzResourceGroup -Name AVRG -Location EastUS`

5. Create a managed availability set by executing the following command:

    `New-AzAvailabilitySet -Location "EastUS" -Name "AVSet" -ResourceGroupName "AVRG" -Sku aligned    -PlatformFaultDomainCount 2 -PlatformUpdateDomainCount 2`

6. Set an administrator username and password for the VMs:

    `$cred = Get-Credential`

7. Create two VMs in the availability set:

    `for ($i=1; $i -le 2; $i++)
{
    New-AzVm 
        -ResourceGroupName "AVRG" -Name "VM$i" -Location "East US" 
        -VirtualNetworkName "Vnet" -SubnetName "Subnet"      -SecurityGroupName "NSG" 
        -PublicIpAddressName "PublicIpAddress$i" 
        -AvailabilitySetName "AVSet" 
        -Credential $cred }`

It takes a few minutes to create and configure both VMs. When finished, you have two virtual machines distributed across the underlying hardware.

If you look at the availability set in the portal by going to **Resource Groups** > **AVRG**  > **AVSet**, you should see how the VMs are distributed across the two fault and update domains.

Once you are done you may want to clean up the resources we created. Use the following PowerShell command to delete the resources we created in this lab:
    `Remove-AzResourceGroup -Name "AVRG"`

# Virtual Machine Durability Labs - Availability Zones
This lab details using Azure PowerShell to create an Azure virtual machine running Windows Server 2016 in an Azure availability zone. An availability zone is a physically separate zone in an Azure region. Use availability zones to protect your apps and data from an unlikely failure or loss of an entire datacenter.

1. Create a resource group:

    `New-AzResourceGroup -Name AvailabilityZoneRG -Location EastUS2`

2. Set an administrator username and password for the VM:

    `$cred = Get-Credential`

3. Build a VM:

    `New-AzVM -ResourceGroupName AvailabilityZoneRG -Location eastus2 -Name AZVM -Size Standard_DS1_v2 -Zone 2 -Credential $cred`

4. Open the Azure Portal, click on **Virtual machines**, then choose **AZVM**.
5. Notice the Location.

Use the following PowerShell command to delete the resources we created in this lab:

`Remove-AzResourceGroup -Name "AvailabilityZoneRG"`