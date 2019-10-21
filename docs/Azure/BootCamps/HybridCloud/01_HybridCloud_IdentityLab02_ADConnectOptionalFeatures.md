# Hybrid Identity Hands-On Lab

## Pass Thru Authentication and High Availability

## Before you Begin

If you are using a Microsoft Azure subscription that was provided to you by Microsoft, you are limited to a specific set of Microsoft Azure regions that you can use. Please use either the **East US, South Central US, West Europe, Southeast Asia, West US 2, or West Central US locations**.
Otherwise you will receive an  error in the portal if you select an unsupported region and attempt to build anything in Microsoft Azure.

## Task 1: Enable the feature

### Enable Pass-through Authentication through Azure AD Connect

1. On your ADConnect VM open **Azure AD Connect**.
2. Select **Configure** then **Change user sign-in task**, and then select **Next**.
3. Logon and then select **Pass-through Authentication** as the sign-in method. Enter the credentials for you AD domain and click **Next**.
4. Click **Configure**. On successful completion, a Pass-through Authentication Agent is installed on the same server as Azure AD Connect and the feature is enabled on your tenant.  Click **Exit** when complete.

**Pass-through Authentication is a tenant-level feature. Turning it on affects the sign-in for users across all the managed domains in your tenant.**

## Task 2: Test Pass-through Authentication

Follow these instructions to verify that you have enabled Pass-through Authentication correctly:

1. Sign in to the Azure Active Directory admin center with the global administrator credentials for your tenant (e.g. adsync).
2. Select **Azure Active Directory** in the left pane.
3. Select **Azure AD Connect**.
4. Verify that the Pass-through authentication feature appears as Enabled.
5. Select Pass-through authentication. The Pass-through authentication pane lists the servers where your Authentication Agents are installed.

## Task 3: Ensure high availability

If you plan to deploy Pass-through Authentication in a production environment, you should install additional standalone Authentication Agents. Install these Authentication Agent(s) on server(s) other than the one running Azure AD Connect. This setup provides you with high availability for user sign-in requests.

### Create a second and third virtual machine

We are creating a small VM to host the Azure AD Connect Authentication Agent.

1. Return to the Azure portal and click the **Create a Resource** button (the Plus) found on the upper left-hand corner of the Azure portal.
2. Select **Compute** then select **Virtual machine**. (Note:  The Azure Portal is intuiative and you may now see a choice named **Windows Server 2016 datacenter** under the **Popular** column.  You can choose this option however be mindful that the steps below are somewhat different.)
3. On the Basics tab complete the following:
    * Resource Group: **AZDCRG**
    * Virtual machine name: **ADConnect2**
    * Region: Choose the same region as your domain controller
    * Availability options: No infrastructure redundancy required
    * Image: Windows Server 2016 Datacenter
    * Size: Choose **DS2_v2**
    * Username: **ADAdmin**
    * Password: `Complex.Password`
    * Confirm Password: `Complex.Password`
    * Public inbound ports: **Allow selected ports**
    * Select inbound ports: **RDP (3389)**
4. Click **Review + create** and then **Create**.   After validation passes, monitor your deployment status. It should take less than 10 minutes to spin up the VM.

### Join ADConnect2 to the domain

1. Connect to the **ADConnect2** virtual machine and logon as ADAdmin. **Microsoft Azure / Resource Groups / AZDCRG / ADConnect2 / Connect.**
2. If prompted, click **No** on the Network discovery blade.
3. Depending on which region you chose for setup, the ADConnect2 virtual machine may or may not have the DNS server set to a value we need.
4. The DNS Server on ADCONNECT2 may not be set to see the domain controller (adVM), so we need to check that setting.  
5. Open a **Command prompt** and enter *ipconfig /all*.  If the DNS Server is set to 10.0.0.4 (the private IP address of adVM), close the Command Prompt window and then continue to **Task 5 - Join the Domain**, otherwise proceed to the **Configure DNS** set of tasks.

### Configure DNS

1. Within **Server Manager**, click on **Local Server**.
2. Click on **IPv4 address assigned by DHCP, IPv6 enabled setting** for the Ethernet connection.
3. Right-click on the network adapter and choose **Properties**.
4. Select **Internet Protocol Version 4 (TCP/IPv4)** and then click **Properties**.
5. Select the radio button for **Use the following DNS Server addresses:** and Set the DNS server to **10.0.0.4** and click **OK** and then **Close**.
6. You will then lose connection to the ADConnect2 virtual machine, this is expected. Once you are back at the Microsoft Azure Portal, click **Restart** to restart the ADConnect VM.
7. Once the VM is successfully restarted, connect to the ADConnect2 virtual machine and logon as ADAdmin.

### Join the Domain

1. Within **Server Manager**, click on **Local Server**.
2. Click on **WORKGROUP**, then **Change** to rename this computer or join it to a domain.
3. Click the radio button for **Domain**, enter your fully-qualified domain name, such as mydomainname.com, and click **OK**.
4. In the Windows Security box enter the AD Domain Admin credentials you specified in the template.
5. Click **Ok** on the Welcome screen, **Ok** on the Computer Name/Domain Changes window, **Close**, then **Restart Now**.

### Repeat the previous steps and create a third VM named **ADConnect3**

### Install the Authentication Agent Software

1. Logon to ADConnect2. Sign in to the Azure Active Directory admin center (aka Azure Portal)  with your tenant's global administrator credentials (ADSYNC).
2. Select **Azure Active Directory** in the left pane.
3. Select **Azure AD Connect**, select **Pass-through authentication**, and then select **Download Agent**.
4. Select the **Accept terms & download button**.
