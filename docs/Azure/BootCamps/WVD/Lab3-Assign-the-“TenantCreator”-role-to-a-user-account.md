# Lab 3: Assign the “Tenant Creator” role to a user account 

Assigning an Azure Active Directory user the Tenant Creator application role allows that user to create a Windows Virtual Desktop tenant associated with the Azure Active Directory instance. You'll need to use your global administrator account to assign the Tenant Creator role. 

During this step you may notice the same account you used in the previous step listed as Default Access, this is not the same as Tenant Creator. So please be sure to complete the steps listed, and repeat if additional administrators will be provisioning WVD Tenants.

https://docs.microsoft.com/en-us/azure/virtual-desktop/tenant-setup-azure-active-directory#assign-the-tenantcreator-application-role

---
**Assigning WVD Tenant Creator Role:**

1. Go to the [Azure portal](https://portal.azure.com), search for and select **Azure Active Directory**.

   ![image.png](/.attachments/image-b6077052-fb44-4a80-bcb7-ea2f38358cb8.png)



2.  In the Middle Pane, click on **Enterprise applications**.

3.  Browes for and select the **Windows Virtual Desktop** application. 

    ![(Not the Windows Virtual Desktop Client Application)](/.attachments/image-0d1676c7-42a1-4311-834f-93e94f496572.png)



4.  In the Middle Pane, select **Users and groups**.



5.  Select **Add user**, select **Users and groups**, and search for the user to
    whom you want to grant permissions to perform the Windows Virtual Desktop
    tenant creation. You should use the account created in the Azure AD section
    and the one you applied Global Admin rights to and should mimic this format,
    myaccountname\@MyAADdomain.onmicrosoft.com

    ![image.png](/.attachments/image-2767bc82-4da0-4e0a-b827-38785ccbb761.png)



6.  Select the user and hit **Select,** followed by **Assign**.

     Your user should now have the role of “TenantCreator.”

   ![image.png](/.attachments/image-cf987e03-1951-4008-aac0-ab46a36392fa.png)



