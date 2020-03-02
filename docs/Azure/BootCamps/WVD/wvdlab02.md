# Lab 2 - Provisioning a WVD tenant

After you establish your network and directory connections, you can provision the Microsoft Azure services required for your Windows Virtual Desktop VMs.

Creating a tenant in Windows Virtual Desktop is the first step toward building your desktop virtualization solution. A tenant is a group of one or more host pools. Each host pool consists of multiple session hosts, running as virtual machines in Azure and registered to the Windows Virtual Desktop service. Each host pool also consists of one or more app groups that are used to publish remote desktop and remote application resources to users. With a tenant, you can build host pools, create app groups, assign users, and make connections through the service.

In this lab you will learn how to:

> * Grant Azure Active Directory permissions to the Windows Virtual Desktop service.
> * Assign the TenantCreator application role to a user in your Azure Active Directory tenant.
> * Create a Windows Virtual Desktop tenant.

## Exercise 1 - Grant permissions to Windows Virtual Desktop

Granting permissions to the Windows Virtual Desktop service lets it query Azure Active Directory for administrative and end-user tasks.

To grant the service permissions:

1. Open a browser and begin the admin consent flow to the [Windows Virtual Desktop server app](https://login.microsoftonline.com/common/adminconsent?client_id=5a0aa725-4958-4b0c-80a9-34562e23f3b7&redirect_uri=https%3A%2F%2Frdweb.wvd.microsoft.com%2FRDWeb%2FConsentCallback).
2. Sign in to the Windows Virtual Desktop consent page with a global administrator account. This would be your adsync@*yourdomain*.onmicrosoft.com.
3. On the **Permissions requested** screen click **Accept**.
4. Wait for one minute so Azure AD can record consent.
5. Open a browser and begin the admin consent flow to the [Windows Virtual Desktop client app](https://login.microsoftonline.com/common/adminconsent?client_id=fa4345a4-a730-4230-84a8-7d9651b86739&redirect_uri=https%3A%2F%2Frdweb.wvd.microsoft.com%2FRDWeb%2FConsentCallback).
6. Sign in to the Windows Virtual Desktop consent page as global administrator, as you did in step 2.
7. On the **Permissions requested** screen click **Accept**.

## Exercise 2 - Assign the TenantCreator application role

Assigning an Azure Active Directory user the TenantCreator application role allows that user to create a Windows Virtual Desktop tenant associated with the Azure Active Directory instance. You'll need to use your global administrator account to assign the TenantCreator role.

To assign the TenantCreator application role:

1. Go to the  [Azure portal](https://portal.azure.com)  to manage the TenantCreator application role. Search for and select **Enterprise applications**. 
    > Don't forget to use the Azure AD you created, not the default directory.
2. Within **Manage** select **Enterprise applications** and then **Windows Virtual Desktop**.
3. Select **Users and groups** under **Manage** You will see that the administrator who granted consent to the application is already listed with the **Default Access** role assigned. 
    > This is not enough to create a Windows Virtual Desktop tenant.
4. Select **Add user**, and then select **Users and groups** in the **Add Assignment** blade.
5. Search for a user account that will create your Windows Virtual Desktop tenant. For simplicity, this can be the global administrator account.
   - If you're using a Microsoft Identity Provider like contosoadmin@live.com or contosoadmin@outlook.com, you might not be able to sign in to Windows Virtual Desktop. We recommend using a domain-specific account like admin@contoso.com or admin@contoso.onmicrosoft.com instead.

   > [!NOTE]
   > You must select a user (or a group that contains a user) that's sourced from this Azure Active Directory instance. You can't choose a guest (B2B) user or a service principal.

6. Select the user account, choose the **Select** button, and then select **Assign**.
7. On the **Windows Virtual Desktop - Users and groups** page, verify that you see a new entry with the **TenantCreator** role assigned to the user who will create the Windows Virtual Desktop tenant.

Before you continue on to create your Windows Virtual Desktop tenant, you need two pieces of information:

   - Your Azure Active Directory tenant ID (or **Directory ID**)
   - Your Azure subscription ID

To find your Azure Active Directory tenant ID (or **Directory ID**):

1. In the same [Azure portal](https://portal.azure.com) session, search for and select **Azure Active Directory**.
2. Scroll down until you find **Properties**, and then select it.
3. Look for **Directory ID**, and then select the clipboard icon. Paste it in a handy location so you can use it later as the **AadTenantId** value.

To find your Azure subscription ID:

1. In the same [Azure portal](https://portal.azure.com) session, search for and select **Subscriptions**.
2. Select the Azure subscription you want to use to receive Windows Virtual Desktop service notifications.
3. Look for **Subscription ID**, and then hover over the value until a clipboard icon appears. Select the clipboard icon and paste it in a handy location so you can use it later as the **AzureSubscriptionId** value.
