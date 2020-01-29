# Azure Bastion Lab

## What is Azure Bastion? (Preview)
  
The Azure Bastion service is a new fully platform-managed PaaS service that you provision inside your virtual network. It provides secure and seamless RDP/SSH connectivity to your virtual machines directly in the Azure portal over SSL. When you connect via Azure Bastion, your virtual machines do not need a public IP address.
Bastion provides secure RDP and SSH connectivity to all VMs in the virtual network in which it is provisioned. Using Azure Bastion protects your virtual machines from exposing RDP/SSH ports to outside world while still providing secure access using RDP/SSH. With Azure Bastion, you connect to the virtual machine directly from the Azure portal. You don't need an additional client, agent, or piece of software.

## Which regions are available during preview

You can deploy and use the Bastion resource to only the **East US** region during the preview with your subscription.

## Exercise 1 - Build Your Networks

1. In the Azure Portal click **+Create New Resource**, then choose **Networking** then **Virtual Networks**.
2. Enter the following and click **Create**:
    * Name:  **BAT_VNet**
    * Address Space: **10.2.0.0/16**
    * Resource Group: *Create New* **Bastion**
    * Location: (US) **East US**
    * Subnet Name: **AzureBastionSubnet**
    * Address Range: **10.2.0.0/27**

## Exercise 2 - Create a bastion host

1. From the home page in the Azure portal, click **+ Create a resource**.
2. On the New page, in the Search the Marketplace field, type **Bastion**, then click Enter to get to the search results.
3. From the results, click **Bastion**. Make sure the publisher is Microsoft and the category is Networking.
4. On the Bastion page, click Create to open the Create a bastion page.
5. On the Create a bastion page, configure a new Bastion resource. Specify the following configuration settings for your Bastion resource:
    * Resource Group: Bastion
    * Name: AzureBat
    * Region: **East US**
    * Virtual network: **BAT_VNet**
    * Subnet: **AzureBastionSubnet**
    * Public IP address:  Prepopulated by default
    * Public IP address name:  Prepopulated by default
    * Public IP address SKU: Prepopulated by default
    * Assignment: Prepopulated by default to Static.
6. Click **Review + create** and then **Create**. It takes about 5 mins for the Bastion resource to be created and deployed.

## Exercise 3 - Create a Virtual Machine in the Bastion Virtual Network

1. Once **AzureBat** is built, head to the home page in the Azure portal and click **+ Create a resource**.
2. Select **Compute** then select **Virtual machine**.
3. On the Basics tab complete the following:
    * Resource Group: **Bastion**
    * Virtual machine name: **BastionVM**
    * Region: **(US) East US**
    * Availability options: No infrastructure redundancy required
    * Image: Windows Server 2016 Datacenter
    * Size: Choose **DS1_v2**
    * Username: *yourfirstname*
    * Password: `Complex.Password`
    * Confirm Password: `Complex.Password`
    * Public inbound ports: **Allow selected ports**
    * Select inbound ports: **RDP (3389)**
4. Click on the **Networking** tab and enter the following:
5. Click **Review + create** and then **Create**.


 virtual machine and then click **Connect**.
7. On the right sidebar, click Bastion, then Use Bastion.
