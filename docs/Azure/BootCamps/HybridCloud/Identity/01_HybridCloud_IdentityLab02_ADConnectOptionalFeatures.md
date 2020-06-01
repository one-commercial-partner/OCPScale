# Hybrid Identity Hands-On Lab

## Pass Thru Authentication and High Availability

## Lab scenario

In this lab you will start by enabling Pass-Thru Authentication (PTA) on your existing Azure AD Connect implementation. Next, you will be provisioning a VM in Azure to run Azure Active Directory Connect.  Afterwards, you will join the virtual machine to the domain and then install the PTA agent.

## Before you Begin

In order to be successful with this lab you will need a clean Azure subscription.  By clean we mean no resources in use or in production and the identity you have associated with this Azure subscription is not associated with any other active Azure subscription or Azure Active Directory.

>DO NOT CONTINUE WITH THIS LAB IF YOU MAY IMPACT PRODUCTIONS RESOURCES AND SERVICES.

You will consume approximately $5 in Azure usage building this lab.  You will also need two (2) Total Regional vCPUs available in your subscription.

## Exercise 1 - Enable the feature

### Enable Pass-through Authentication through Azure AD Connect

1. On your ADConnect VM open **Azure AD Connect**.
2. Select **Configure** then the **Change user sign-in** task, and then select **Next**.
3. Logon, select **Pass-through Authentication** as the sign-in method, and click **Next**.
4. Click on **Enter credentials** for your AD domain and click **Next**.
5. Click **Configure**. On successful completion, a Pass-through Authentication Agent is installed on the Azure AD Connect virtual machine and the feature is enabled on your tenant.  
6. Click **Exit** when complete.

## Exercise 2 - Test Pass-through Authentication

Follow these instructions to verify that you have enabled Pass-through Authentication correctly:

1. Sign in to the Azure Active Directory admin center (<https://aad.portal.azure.com/>) with the global administrator credentials for your tenant (e.g. adsync@*yourdomain*.onmicrosoft.com).
2. Select **Azure Active Directory** in the left pane under **Favorites**.
3. Select **Azure AD Connect** under **Manage**.
4. Verify that the Pass-through authentication feature appears as Enabled under **User Sign-In**.
5. Select **Pass-through authentication**. The Pass-through authentication pane lists the servers where your Authentication Agents are installed.

## Exercise 3 - Create a second virtual machine

We are creating a small VM to host the Azure AD Connect Authentication Agent.

1. Open an Azure CLI window by browsing to [Azure Shell](https://shell.azure.com).
2. If prompted, reconnect and login using your Microsoft Azure Account.
3. Create your virtual machine:.

    ```PowerShell
    az vm create --resource-group Identity-Infrastructure --name ADConnect2 --size Standard_B2s --image Win2019Datacenter --admin-username ADadmin --admin-password Complex.Password --nsg AD-NSG --private-ip-address 10.10.10.12
    ```

## Exercise 4 - Join ADConnect2 to the domain

1. Once the virtual machine is successfully provisioned, connect to the ADConnect2 virtual machine and logon as ADAdmin.
2. Within **Server Manager**, click on **Local Server**.
3. Click on **WORKGROUP**, then **Change** to rename this computer or join it to a domain.
4. Click the radio button for **Domain**, enter your fully-qualified domain name, such as mydomainname.com, and click **OK**.
5. In the Windows Security box enter the AD Domain Admin credentials you wrote down earlier.
6. Click **Ok** on the Welcome screen, **Ok** on the Computer Name/Domain Changes window, **Close**, then **Restart Now**.

## Exercise 5 - Install the Authentication Agent Software

1. Connect to the ADConnect2 virtual machine and logon as your domain account `domain\ADAdmin`.
2. When **Server Manager** opens select **Local Server** and turn off **IE Enhanced Security Configuration** for Administrators and Users.
3. Sign in to the [Azure Active Directory admin center](https://aad.portal.azure.com/)  with your tenant's global administrator credentials (AzureADSync@*yourAzureADdomain*.onmicrosoft.com).
4. Select **Azure Active Directory** in the left pane.
5. Uder **manage**, select **Azure AD Connect**, click on **Pass-through authentication**, and then select **Download**.
6. Install the client:
    * Select the **Accept terms & download** button and then click **Run**. Accept the default values for installation.
    * When prompted enter your adsync credentials.
    * Click **Close** when complete.
7. Click **Refresh** on the Azure portal.  You'll notice that ADConnect2 now appears in the default group.  If you receive a status of **Inactive** wait a few moments and refresh the portal.
