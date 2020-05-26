# Lab 2 - Load Balancer

Load balancing provides a higher level of availability and scale by spreading incoming requests across multiple virtual machines (VMs). You can use the Azure portal to create a load balancer that will load balance virtual machines. In this lab you will learn how to create network resources, back-end servers, and a load balancer.

## Exercise 1 - Create a Standard load balancer

In this section, you create a public standard load balancer by using the portal. The public IP address is automatically configured as the load balancer's front end when you create the public IP and the load balancer resource by using the portal. The name of the front end is myLoadBalancer.

1. On the upper-left side of the portal, select **Microsoft Azure** and then  **+Create a resource** > **Networking** > **Load Balancer**.
2. In the Create load balancer pane, enter these values:
    * Resource Group: *Create New* **LoadBalancers**
    * Name: **LB01**
    * Region: Use the same region as your other resources
    * Type: **Public**
    * SKU: **Basic**
    * Public IP address: **Create New**
    * Public IP address name: **LBPublicIP**
    * Select **Review + Create**.
    * After Validation passes click **Create**.

## Exercise 2 - Create back-end servers

In this section you create a virtual network and you create two virtual machines for the back-end pool of your Basic load balancer. Then you install Internet Information Services (IIS) on the virtual machines to help test the load balancer.

### Create a virtual network

1. On the upper-left side of the portal, select **Create a resource** > **Networking** > **Virtual network**.
2. In the Create virtual network pane enter these values:
    * **LoadBalVMs** for the name of the resource group (*Create new*)
    * **LBvNet** for the Instance Name of the virtual network
    * **Region**: Use the same region as your other resources
    * Click **Next:  IP Addresses >**
3. On the IP Addresses pane complete the following:
    * Click the trash can to delete the existing IPv4 address space
    * Enter **10.20.0.0/16** as the address space
    * Click on **+ Add subnet**
    * Enter **Backend** for the subnet name
    * Enter **10.20.0.0/24** as the address range
    * Click **Add**
4. Click **Review + Create** and after validation passes, click **Create**.

### Create LBVM1

Provision a virtual machine via Cloud Shell.

1. Click on the Cloud Shell icon on the taskbar: >_
2. If not already, change the shell type to PowerShell.
3. If you are prompted, select Create Storage.
4. Enter the following to set the username and password needed for the administrator account on the VM. Enter yourfirstname as the User and then Complex.Password as the password.

    ```powershell
    $cred = Get-Credential
    ```

5. Create an availability set.

    > NOTE: Ensure you are using the right region!

    ```powershell
    New-AzAvailabilitySet -ResourceGroupName "LoadBalVMs" -Name "LBVMAVSet" -Location "EastUS" -Sku "Aligned" -PlatformFaultDomainCount 2 -PlatformUpdateDomainCount 3
6. Create the VM (note to use the correct region):
    ```powershell
    New-AzVm -ResourceGroupName "LoadBalVMs" -Name "LBVM1" -Location "EastUS" -VirtualNetworkName "LBvNet" -SubnetName "Backend" -SecurityGroupName "LBVM1-nsg" -PublicIpAddressName "LBVM1-ip" -Credential $cred -size Standard_DS2_v2 -AvailabilitySetName "LBVMAVSet"
    ```
    > Note that it may take 5-8 minutes to provision the virtual machine.

### Create LBVM2

1. Create the VM:

    > NOTE: Ensure you are using the right region!

    ```powershell
    New-AzVm -ResourceGroupName "LoadBalVMs" -Name "LBVM2" -Location "EastUS" -VirtualNetworkName "LBvNet" -SubnetName "Backend" -SecurityGroupName "LBVM2-nsg" -PublicIpAddressName "LBVM2-ip" -Credential $cred -size Standard_DS2_v2 -AvailabilitySetName "LBVMAVSet"
    ```

    > Note that it may take 5-8 minutes to provision the virtual machine.

## Exercise 3 - Create NSG rules

In this section, you create NSG rules to allow inbound connections that use HTTP and RDP.

1. Select **Resource Groups** in the Azure Portal and from the resource list select **LoadBalVMs** then **LBVM1-nsg**.
2. Under **Settings**, select **Inbound security rules**, and then select **+Add**.
3. Enter the following values for the inbound security rule named **HTTP-In** to allow for inbound HTTP connections that use port 80.
    * Source: **Service Tag**
    * Source service tag: **Internet**
    * Source port ranges: *****
    * Destination: **Any**
    * Destination port ranges: **80**
    * Protocol: **TCP**
    * Action: **Allow**
    * Priority: **100**
    * Name: **HTTP-In**
    * Description: **Allow HTTP**
    * Click **Add**

4. Repeat the above steps except from the resource list, select **LoadBalVMs** then **LBVM2-nsg**.

## Exercise 4 - Install IIS

1. Select **Virtual Machines** in the Azure Portal and then select **LBVM1**.
2. On the Overview page, select **Connect** to RDP into the VM.  Download and open the RDP file if needed.
3. Sign in to the VM with your username and password.  Don't forget you may need to select **More choices** and then **Use a different account**.
4. Click **No** on the Networks blade.
5. Open **Windows Powershell** and enter the following:

    `Install-WindowsFeature -name "Web-Server" -IncludeManagementTools`

### Edit the default web page

Once IIS is installed edit the default web page by:

* In Server Manager, click **Tools** then **IIS Manager**
* Expand the left tree, right-click on Default web site, and then choose **Explore**
* Edit the iisstart.html by opening it with Notepad.
* Change the `<title>` line to read: `<title>LBVM1</title>`
* Save the file and then exit your RDP connection

Repeat the same steps for LBVM2 and change the `<title>` line to state `<title>LBVM2</title>`

## Exercise 5 -  Create resources for the load balancer

In this section, you configure load balancer settings for a back-end address pool and a health probe. You also specify load balancer and NAT rules.

### Task 1 - Create a Backend pool

1. On left side of the Azure Portal click on the portal menu icon, select **Load Balancers**, then select **LB01**. Under **Settings**, select **Backend pools** then **+ Add**.
2. Enter the following information:
    * Name: **BackendPool**
    * Virtual Network: LBvNet
    * Associated to: **Virtual Machines**
    * Click **+ Add**, select LBVM1 and LBVM2, and then click **Add**
    * Click **Add**

### Task 2 - Create a health probe

To allow the load balancer to monitor the status of your app, you use a health probe. The health probe dynamically adds or removes VMs from the load balancer rotation based on their response to health checks. Create a health probe to monitor the health of the VMs.

1. Under **Settings**, select **Health probes**, and then select **+ Add**.
2. Enter these values and then select **OK**:
    * Name: **LBHealthProbe**
    * Protocol: **HTTP**
    * Port: **80** for the  number
    * Path: **/**
    * Interval: **5** for the number of seconds between probe attempts
    * Unhealthy threshold: **2** for the number of consecutive probe failures that must occur before a VM is considered unhealthy

### Task 3 - Create a load balancer rule

1. Under **Settings**, select **Load balancing rules**, and then select **+ Add**.
2. Enter these values and then select **OK**:
    * **HTTPRule** for the name of the load balancer rule
    * **TCP** for the protocol type
    * **80** for the port number
    * **80** for the back-end port
    * **BackendPool** for the name of the back-end pool
    * **LBHealthProbe** for the name of the health probe

## Exercise 6 -  Test the load balancer

1. Find the public IP address for the load balancer on the Load Balancer Overview screen.
2. Copy the public IP address and then paste it into the address bar of your browser. Connect to the IP address and notice the default page of the IIS web server is displayed in the browser, noting LBVM1 or LBVM2 as you refresh your browser.
3. Stop either LBVM1 or LBVM2, whichever VM is responding most frequently.  As the VM is shutting down, refresh your browser.  Once one of the VMs is down, you should only see the live VM rendering you the default website.  You may receive a service unavailable error if you refresh during probe attempts.
4. Stop the other LBVM and refresh your browser until you receive a **Can't reach this page** error and then restart the first VM you stopped.
5. Once the VM comes alive you should get responses.
6. Alternately, you can always check the load balancer status by selecting **Load Balancers**, **Backend Pools**, then expand the **virtual machine pool**.
