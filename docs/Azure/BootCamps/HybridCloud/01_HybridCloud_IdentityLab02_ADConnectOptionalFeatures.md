# Hybrid Identity Hands-On Lab

## Pass Thru Authentication and High Availability

### Complete this lab if time permits

## Before you Begin

If you are using a Microsoft Azure subscription that was provided to you by Microsoft, you are limited to a specific set of Microsoft Azure regions that you can use. Please use either the **East US, South Central US, West Europe, Southeast Asia, West US 2, or West Central US locations**.
Otherwise you will receive an  error in the portal if you select an unsupported region and attempt to build anything in Microsoft Azure.

## Exercise 1 - Enable the feature

### Enable Pass-through Authentication through Azure AD Connect

1. On your ADConnect VM open **Azure AD Connect**.
2. Select **Configure** then the **Change user sign-in** task, and then select **Next**.
3. Logon, select **Pass-through Authentication** as the sign-in method, and click **Next**.
4. **Enter credentials** for your AD domain and click **Next**.
5. Click **Configure**. On successful completion, a Pass-through Authentication Agent is installed on the  Azure AD Connect virtual machine and the feature is enabled on your tenant.  
6. Click **Exit** when complete.

**Pass-through Authentication is a tenant-level feature. Turning it on affects the sign-in for users across all the managed domains in your tenant.**

## Exercise 2 - Test Pass-through Authentication

Follow these instructions to verify that you have enabled Pass-through Authentication correctly:

1. Sign in to the Azure Active Directory admin center with the global administrator credentials for your tenant (e.g. adsync).
2. Select **Azure Active Directory** in the left pane.
3. Select **Azure AD Connect**.
4. Verify that the Pass-through authentication feature appears as Enabled under **User Sign-In**.
5. Select **Pass-through authentication**. The Pass-through authentication pane lists the servers where your Authentication Agents are installed.

## Exercise 3 - Ensure high availability

If you plan to deploy Pass-through Authentication in a production environment, you should install additional standalone Authentication Agents. Install these Authentication Agent(s) on server(s) other than the one running Azure AD Connect. This setup provides you with high availability for user sign-in requests.

## Exercise 4 - Create a second virtual machine

We are creating a small VM to host the Azure AD Connect Authentication Agent.

1. Return to the Azure portal and click the **Create a Resource** button (the Plus) found on the upper left-hand corner of the Azure portal.  *Make sure that you are in the correct directory!*
2. Select **Compute** then select **Virtual machine**. (Note:  The Azure Portal is intuitive and you may now see a choice named **Windows Server 2016 datacenter** under the **Popular** column.  You can choose this option however be mindful that the steps below are somewhat different.)
3. On the Basics tab complete the following:
    * Resource Group: **AD-ResourceGroup**
    * Virtual machine name: **ADConnect2**
    * Region: Choose the same region as your domain controller
    * Availability options: No infrastructure redundancy required
    * Image: Windows Server 2016 Datacenter
    * Size: Choose **Standard D2 v3**
    * Username: **ADAdmin**
    * Password: `Complex.Password`
    * Confirm Password: `Complex.Password`
    * Public inbound ports: **Allow selected ports**
    * Select inbound ports: **RDP (3389)**
4. Click **Review + create**.   After validation passes click **Create** and then monitor your deployment status. It should take less than 10 minutes to spin up the VM.

## Exercise 5 - Join ADConnect2 to the domain

1. Once the **ADConnect2** virtual machine is up and running, logon as ADAdmin. **Microsoft Azure / Resource Groups / AD-ResourceGroup / ADConnect2 / Connect.**
2. When prompted click **No** on the Network discovery blade.
3. Depending on which region you chose for setup, the ADConnect2 virtual machine may or may not have the DNS server set to a value we need.
4. Open a **Command prompt** and enter *ipconfig /all*.  If the DNS Server is set to **10.10.10.11**  (the private IP address of DC01), close the Command Prompt window and then continue to **Exercise 6 - Join the Domain**, otherwise proceed to the **Configure DNS** set of tasks.

### Configure DNS

1. Within **Server Manager**, click on **Local Server**.
2. Click on **IPv4 address assigned by DHCP, IPv6 enabled setting** for the Ethernet connection.
3. Right-click on the network adapter and choose **Properties**.
4. Select **Internet Protocol Version 4 (TCP/IPv4)** and then click **Properties**.
5. Select the radio button for **Use the following DNS Server addresses:** and Set the DNS server to **10.10.10.11**  and click **OK** and then **Close**.
6. You will then lose connection to the ADConnect2 virtual machine, this is expected. Once you are back at the Microsoft Azure Portal, click **Restart** to restart the virtual machine.

## Exercise 6 - Join the Domain

1. Once the virtual machine is successfully restarted, connect to the ADConnect2 virtual machine and logon as ADAdmin.
2. Within **Server Manager**, click on **Local Server**.
3. Click on **WORKGROUP**, then **Change** to rename this computer or join it to a domain.
4. Click the radio button for **Domain**, enter your fully-qualified domain name, such as mydomainname.com, and click **OK**.
5. In the Windows Security box enter the AD Domain Admin credentials you wrote down earlier.
6. Click **Ok** on the Welcome screen, **Ok** on the Computer Name/Domain Changes window, **Close**, then **Restart Now**.

## Exercise 7 - Install the Authentication Agent Software

1. Connect to the ADConnect2 virtual machine and logon as your domain account (*domain\username*).
2. When **Server Manager** opens select **Local Server** and turn off **IE Enhanced Security Configuration** for Administrators and Users.
3. Sign in to the Azure Active Directory admin center (<http://portal.azure.com)>  with your tenant's global administrator credentials (ADSYNC@*yourdomain*.onmicrosoft.com).
4. Select **Azure Active Directory** in the left pane.
5. Select **Azure AD Connect**, select **Pass-through authentication**, and then select **Download**.
6. Install the client:
    * Select the **Accept terms & download** button and then click **Run**. Accept the default values for installation.
    * When prompted enter your adsync credentials.
    * Click **Close** when complete.
7. Click **Refresh** on the Azure portal.  You'll notice that ADConnect2 now appears in the default group.  If you receive a status of **Inactive** wait a few moments and refresh the portal.
