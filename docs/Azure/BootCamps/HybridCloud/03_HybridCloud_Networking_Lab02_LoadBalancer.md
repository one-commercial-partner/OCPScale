# Lab 2 - Load Balancer

Load balancing provides a higher level of availability and scale by spreading incoming requests across multiple virtual machines (VMs). You can use the Azure portal to create a load balancer that will load balance virtual machines. In this lab you will learn how to create network resources, back-end servers, and a load balancer.

## Exercise 1 - Create a Standard load balancer

In this section, you create a public standard load balancer by using the portal. The public IP address is automatically configured as the load balancer's front end when you create the public IP and the load balancer resource by using the portal. The name of the front end is myLoadBalancer.

1. On the upper-left side of the portal, select **+Create a resource** > **Networking** > **Load Balancer**.
2. In the Create load balancer pane, enter these values:
    * Resource Group: *Create New* **LoadBalancers**
    * Name: **LB01**
    * Region: Use the same region as your other resources
    * Type: **Public**
    * SKU: **Standard**
    * Public IP address: **Create New**
    * Public IP address name: **LBPublicIP**
    * Availability Zone: **Zone-Redundant**
    * Select **Review + Create**.
    * After Validation passes click **Create**.

## Exercise 2 - Create back-end servers

In this section you create a virtual network and you create two virtual machines for the back-end pool of your Basic load balancer. Then you install Internet Information Services (IIS) on the virtual machines to help test the load balancer.

### Create a virtual network

1. On the upper-left side of the portal, select **Create a resource** > **Networking** > **Virtual network**.
2. In the Create virtual network pane enter these values:
    * **LBvNet** for the name of the virtual network
    * **10.20.0.0/16** as the address space
    * **LoadBalVMs** for the name of the resource group (*Create new*)
    * **Location**: Use the same region as your other resources
    * **Backend** for the subnet name
    * **10.20.0.0/24** as the address space
    * Click **Create**.

### Create LBVM1

1. On the upper-left side of the portal, select **Create a resource** > **Compute** > **Windows Server 2016 Datacenter**.
2. Enter, or select, the following information, accept the defaults for the remaining settings:
    * Resource Group: **LoadBalVMs**
    * Name: **LBVM1**
    * Region: *Choose a consistent supported Region*
    * Availability option: choose **Availability set** > **Create New** > **LBVMAVSet** > **Ok**.
    * Size: Set to **DS2 v2**
    * Username: pick a username
    * Password: `Complex.Password`
    * Confirm Password: `Complex.Password`
    * Public inbound ports: Open RDP, 3389
    * Select the **Networking** tab
    * Configure the following settings:
        * Virtual Network: **LBvNet**
        * Subnet: **Backend**
        * Public IP: *Create New*
            * Name: **LBVM1-ip**
            * SKU: **Standard**
            * Click **Ok**
            * **Please make sure that you click **Create new** in order to set the SKU correctly**
    * Place this virtual machine behind an existing Azure load balancing solution: **Yes**
        * Load balancing options: **Azure load balancer**
        * Select a load balancer: **LB01**
        * Select a backend pool: *Create new* **BackendPool**
        * Select **Create** and then **Review + create**.
    * Once validation passes click **Create**.

### Create LBVM2

1. By following the previous steps create a second VM:
    * Name: **LBVM2**
    * **LBVMAVSet** as the existing availability set.
    * **LBvNet** as the virtual network.
    * **Backend** as the subnet.
    * Public IP: *Create New*
        * Name: **LBVM2-ip**
        * SKU: **Standard**
        * Click **Ok**

## Exercise 3 - Create NSG rules

In this section, you create NSG rules to allow inbound connections that use HTTP and RDP.

1. Select **Resource Groups** in the Azure Portal and from the resource list select **LoadBalVMs** then **LBVM1-nsg**.
2. Under **Settings**, select **Inbound security rules**, and then select **+Add**.
3. Select **Add** and then enter the following values for the inbound security rule named **HTTP-In** to allow for inbound HTTP connections that use port 80.
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

1. Select **Virtual Machines** in the Azure Portla and then select **LBVM1**.
2. On the Overview page, select **Connect** to RDP into the VM.  Download and open the RDP file if needed.
3. Sign in to the VM with your username and password.  Don't forget you may need to select **More choices** and then **Use a different account**.
4. Click **No** on the Networks blade.
5. Open **Windows Powershell** and enter the following:

    `Install-WindowsFeature -name "Web-Server" -IncludeManagementTools`

Repeat steps 1 to 5 for the virtual machine LBVM2.

Once IIS is installed, on each VM edit the default web page by:

* In Server Manager, click **Tools** then **IIS Manager**
* Expand the left tree, right-click on Default web site, and then choose **Explore**
* Edit the iisstart.html by opening it with Notepad.
* Change the `<title>` line to read: `<title>LBVM1</title>`
* Save the file and then exit your RDP connection

Repeat the same steps for LBVM2 and change the `<title>` line to state `<title>LBVM2</title>`

## Exercise 5 -  Create resources for the load balancer

In this section, you configure load balancer settings for a back-end address pool and a health probe. You also specify load balancer and NAT rules.

### Create a health probe

To allow the load balancer to monitor the status of your app, you use a health probe. The health probe dynamically adds or removes VMs from the load balancer rotation based on their response to health checks. Create a health probe named LBHP to monitor the health of the VMs.

1. On left side of the Azure Portal select **Load Balancers** and then selelct **LB01**. Under **Settings**, select **Health probes**, and then select **+Add**.
2. Enter these values and then select **OK**:
    * **LBHealthProbe** for the name of the health probe
    * **HTTP** for the protocol type
    * **80** for the port number
    * **5** for Interval, the number of seconds between probe attempts
    * **2** for Unhealthy threshold, the number of consecutive probe failures that must occur before a VM is considered unhealthy

### Create a load balancer rule

You use a load balancer rule to define how traffic is distributed to the VMs. You define the frontend IP configuration for the incoming traffic and the back-end IP pool to receive the traffic, along with the required source and destination port.

Create a load balancer rule named HTTPRule for listening to port 80 in the front end LoadBalancerFrontEnd. The rule is also for sending load-balanced network traffic to the back-end address pool myBackEndPool, also by using port 80.

1. Under **Settings**, select **Load balancing rules**, and then select **+Add**.
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
