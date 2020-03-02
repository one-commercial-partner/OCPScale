# Lab 2 - WVD Core Infrastructure

Creating a tenant in Windows Virtual Desktop is the first step toward building your desktop virtualization solution. A tenant is a group of one or more host pools. Each host pool consists of multiple session hosts, running as virtual machines in Azure and registered to the Windows Virtual Desktop service. Each host pool also consists of one or more app groups that are used to publish remote desktop and remote application resources to users. With a tenant, you can build host pools, create app groups, assign users, and make connections through the service.

In this lab you will learn how to:

> * Grant Azure Active Directory permissions to the Windows Virtual Desktop service.
> * Assign the TenantCreator application role to a user in your Azure Active Directory tenant.
> * Create a Windows Virtual Desktop tenant.

## Exercise 1 - Grant permissions to Windows Virtual Desktop

Granting permissions to the Windows Virtual Desktop service lets it query Azure Active Directory for administrative and end-user tasks.

To grant the service permissions:

1. Open a browser and begin the admin consent flow to the [Windows Virtual Desktop server app](https://login.microsoftonline.com/common/adminconsent?client_id=5a0aa725-4958-4b0c-80a9-34562e23f3b7&redirect_uri=https%3A%2F%2Frdweb.wvd.microsoft.com%2FRDWeb%2FConsentCallback).
2. Sign in to the Windows Virtual Desktop consent page with a global administrator account. For example, if you were with the Contoso organization, your account might be admin@contoso.com or admin@contoso.onmicrosoft.com.
3. Select **Accept**.
4. Wait for one minute so Azure AD can record consent.
5. Open a browser and begin the admin consent flow to the [Windows Virtual Desktop client app](https://login.microsoftonline.com/common/adminconsent?client_id=fa4345a4-a730-4230-84a8-7d9651b86739&redirect_uri=https%3A%2F%2Frdweb.wvd.microsoft.com%2FRDWeb%2FConsentCallback).
6. Sign in to the Windows Virtual Desktop consent page as global administrator, as you did in step 2.
7. Select **Accept**.