# Lab 4: Grant consent for the Windows Virtual Desktop service

Creating a tenant in Windows Virtual Desktop is the first step toward building your desktop virtualization solution. A Windows Virtual Desktop tenant is a group of one or more host pools. With a Windows Virtual Desktop  tenant, you can build host pools, create app groups, assign users, and make connections through the service.

Before you can create a Windows Virtual Desktop tenant, you must allow Windows Virtual Desktop services to access your Azure AD tenant. The way Windows Virtual Desktop is designed requires explicit Azure AD consent. The process is much like how Azure requires you to enable non-standard resource providers before being able to use them.

## Exercise 1 - Enabling Server Registration and consent to Azure AD

1. On your desktop browse to the URL: [Windows Virtual Desktop Consent Page](https://rdweb.wvd.microsoft.com/).  
2. Under **Consent Option** select **Server App** from the dropdown and then paste your Azure Active Directory ID (your Tenant ID) into the **AAD Tenant GUID or Name:** and click **Submit**.
3. When prompted, select **Use another account** and then enter  `AzureADAdmin@<yourAzureADDomain>.onmicrosoft.com`.  Click **Next**.

4. Enter `Complex.Password` and then **Sign in**.

5. Click **Accept** on the **Permissions requested** page.

## Exercise 2 - Enabling Client Registration and consent to Azure AD

1. **WAIT ONE MINUTE** before proceeding. It can take about a minute for the server registration to complete. If you proceed to the next step too quickly, you may see an error.
2. Navigate back to [Windows Virtual Desktop Consent Page](https://rdweb.wvd.microsoft.com/).
3. Next, from the **Consent Option** drop down, select **Client App**  and add the same Azure Active Directory ID (your Tenant ID) into the **AAD Tenant GUID or Name:** and click **Submit**.
4. When prompted `AzureADAdmin@<yourAzureADDomain>.onmicrosoft.com`.
5. Click **Accept** on the permissions request page. Once this is complete you can close out of your browser session.

    >**NOTE**: If you receive an error that the page is not working or not available right now, click the button in your browser to go back one page and try again.  This is a temporary condition.

### Continue with Lab 5: [Assign the “TenantCreator” role](Prepare-Lab05-Assign-the-“TenantCreator”-role-to-a-user-account.md)

### Return to [Prepare Phase Labs](prepare.md)
