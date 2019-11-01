# Azure Bastion Lab

## What is Azure Bastion? (Preview)
  
The Azure Bastion service is a new fully platform-managed PaaS service that you provision inside your virtual network. It provides secure and seamless RDP/SSH connectivity to your virtual machines directly in the Azure portal over SSL. When you connect via Azure Bastion, your virtual machines do not need a public IP address.
Bastion provides secure RDP and SSH connectivity to all VMs in the virtual network in which it is provisioned. Using Azure Bastion protects your virtual machines from exposing RDP/SSH ports to outside world while still providing secure access using RDP/SSH. With Azure Bastion, you connect to the virtual machine directly from the Azure portal. You don't need an additional client, agent, or piece of software.

## Which regions are available during preview

You can deploy and use the Bastion resource to only the **East US** region during the preview with your subscription.

## Create a bastion host using VM settings

We are creating a small VM to be used as an Azure Bastion Host.

1. In Azure Preview portal for Azure Bastion  [Azure portal - preview link](https://aka.ms/BastionHost). `Make sure you use the link provided to access the portal for this preview, not the regular Azure portal.`
2. Click the **Create a Resource** button (the Plus) found on the upper left-hand corner.
3. Select **Compute** then select **Virtual machine**.
4. On the Basics tab complete the following:
    * Resource Group: **Bastion**
    * Virtual machine name: **BastionVM**
    * Region: **(US) West US**
    * Availability options: No infrastructure redundancy required
    * Image: Windows Server 2016 Datacenter
    * Size: Choose **Standard DS1 v2**
    * Username: **enter your first name**
    * Password: `Complex.Password`
    * Confirm Password: `Complex.Password`
    * Public inbound ports: **Allow selected ports**
    * Select inbound ports: **RDP (3389)**
    * Click **Next:Disks >**
    * Click **Next: Networking >**
        * Virtual network: (create new) **BastionVnet**
        * Click the box next to 10.0.0.0/24
        * Under Subnets:
            * Change the name of the default subnet to  **AzureBastionSubnet** and click the box next to **Subnet name**
        * Click **OK**
5. Click **Review + create** and then **Create**.   After validation passes, monitor your deployment status.
6. Once BastionVM is built, navigate to your virtual machine and then click Connect.

On the right sidebar, click Bastion, then Use Bastion.

On the Bastion page, fill out the following settings fields:
Name: The name of the bastion host you want to create.
Subnet: The subnet inside your virtual network to which Bastion resource will be deployed. The subnet must be created with the name AzureBastionSubnet. This lets Azure know which subnet to deploy the Bastion resource to. This is different than a Gateway subnet. Click Manage subnet configuration to create the Azure Bastion Subnet. We highly recommend that you use at least a /27 or larger subnet (/27, /26, etc.). Create the AzureBastionSubnet without any Network Security Groups, route tables, or delegations. Click Create to create the subnet, then proceed with the next settings.

Public IP address: The public IP of the Bastion resource on which RDP/SSH will be accessed (over port 443). Create a new public IP, or use an existing one. The public IP address must be in the same region as the Bastion resource you are creating.
Public IP address name: The name of the public IP address resource.
On the validation screen, click Create. Wait for about 5 mins for the Bastion resource to be created and deployed.

## Create a bastion host

This section helps you create a new Azure Bastion resource from the Azure portal.

1. From a web browser click [Azure portal - preview link](https://aka.ms/BastionHost). `Make sure you use the link provided to access the portal for this preview, not the regular Azure portal`.
2. In the Search bar type **Bastion**, then click Enter to get to the search results.
3. From the results, click **Bastion**.
4. On the Create a bastion page, configure a new Bastion resource. Specify the configuration settings for your Bastion resource:
    * Resource Group: (create new) **Bastion**
    * Name: **BastionVM**
    * Region: **East US**
    * Virtual network: (create new) **BastionVnet**
        * Click the box next to 10.0.0.0/16
        * Under Subnets:
            * Change the name of the default subnet to  **AzureBastionSubnet**
            * Click the box next to **Subnet name**
        * Click **OK**
    * When you have finished specifying the settings, click **Review + Create**. This validates the values. Once validation passes, you can begin the creation process.
    * On the Create a bastion page, click **Create**.
