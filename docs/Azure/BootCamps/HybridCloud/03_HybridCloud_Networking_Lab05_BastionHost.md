# Azure Bastion Lab

## What is Azure Bastion? (Preview)
  
The Azure Bastion service is a new fully platform-managed PaaS service that you provision inside your virtual network. It provides secure and seamless RDP/SSH connectivity to your virtual machines directly in the Azure portal over SSL. When you connect via Azure Bastion, your virtual machines do not need a public IP address.
Bastion provides secure RDP and SSH connectivity to all VMs in the virtual network in which it is provisioned. Using Azure Bastion protects your virtual machines from exposing RDP/SSH ports to outside world while still providing secure access using RDP/SSH. With Azure Bastion, you connect to the virtual machine directly from the Azure portal. You don't need an additional client, agent, or piece of software.

## Which regions are available during preview

You can deploy and use the Bastion resource to only the **East US** region during the preview with your subscription.

## Leverage the QuickStart Template

We're going to leverage a quickstart template to make the build process quick and easy.  This template will deploy Azure Bastion in a new or existing Azure Virtual Network, along with dependent resources such as the AzureBastionSubnet, Public Ip Address for Azure Bastion, and Network Security Group rules.
This template deploys resources in the same Resource Group and Azure region as the Virtual Network.

1. Open a web browser and surf to [Deploy Azure Bastion in an Azure Virtual Network](https://github.com/Azure/azure-quickstart-templates/tree/master/101-azure-bastion).
2. Click on **Deploy to Azure**.
3. Enter the following:
    * Resource Group (create new): **Bastion**
    * Location: **(US) East US**
    * Vnet-name: **BastionVnet**
    * Bastion-host-name: **BastionHost**
4. Click the box for **I agree ...** and the click **Purchase**.
5. Monitor the deployment.  It will take about 5 minutes to spin everything up.

## Build a VM in the Bastion VNet

1. Once **BastionHost** is built, open BastionHost and the click **+Add**.
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
4. CLick on the **Networking** tab and enter the following:
    
4. Click **Review + create** and then **Create**.


 virtual machine and then click **Connect**.
7. On the right sidebar, click Bastion, then Use Bastion.
