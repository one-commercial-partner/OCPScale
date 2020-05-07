# Lab 3: Configuring Azure AD Connect with AD DS

In this exercise you will be configuring [Azure AD Connect](https://docs.microsoft.com/en-us/azure/active-directory/hybrid/whatis-azure-ad-connect). With Windows Virtual Desktop, all session host VMs within the WVD tenant environment are required to be domain joined to AD DS, and the domain must be synchronized with Azure AD. To manage the synchronization of objects, you will configure Azure AD Connect on the domain controller deployed in Azure.

> *Note:* RDP access to a domain controller using a public IP address is not a best practice and is only done to simplify this lab. Better security practices such as removing the PIP, enabling just-in-time access and/or leveraging a bastion host should be applied enhance security.

---

## Exercise 1 - Install Azure Active Directory

1. In the Azure Portal, click **Microsoft Azure** and then **+Create a resource**.  Select **Identity** and then **Azure Active Directory**.
2. Enter the following on the **Create directory** tab:
    * Organization name: **WVD Lab**
    * Initial domain name: `<yourinitials>`WVDLab *(e.g. abdWVDLab)*
    * Hit **Tab**.

        >Ensure validation passes as your namespace needs to be unique within the onmicrosoft.com namespace.  We often see students choosing a domain name that already exists.

3. Click **Create**.  It will take several minutes for the directory to be created.
4. Once complete, click **here** to manage your new directory.

    ![ClickHere](../attachments/ClickHere.PNG)

5. Copy the Azure Active Directory Domain Name and Tenant ID to a scratch location such as Notepad.

### Create a Global Account

1. Under **Manage** select **Users**.
2. Click on **+ New user**.
3. Enter the following:
    * User name: **AzureADAdmin**
    * Name: **AzureADAdmin**
    * Under Password:
        * Select **Let me create the password**
        * Initial password: `Temporary.Password`
    * Under **Groups and roles** click on **User**
        * Scroll down to find and select `Global administrator`.
        * Click **Select**
    * Usage location: **United States**
4. Click **Create**.

### Reset the password

1. Open an InPrivate or Incognito browser.
2. Surf to portal.azure.com.
3. Logon as `AzureADAdmin@<yourAzureADdomain>.onmicrosoft.com` with a password of `Temporary.Password`.
4. Update your password to `Complex.Password`.
5. Close the InPrivate or Incognito browser.

## Exercise 2 - Create a Sync Account

We are going to create an account that AD Connect will use to perform the synchronization process bethween the on-prem domain controller and Azure Active Directory.

1. In Azure Active Directory click on **+ New User** and enter the following:
    * User name: **AzureADSync**
    * Under Password:
        * Select **Let me create the password**
        * Initial password: `Temporary.Password`
    * Under **Groups and roles** click on **User**
        * Scroll down to find and select `Global administrator`.
        * Click **Select**
    * Usage location: **United States**
2. Click **Create**.

### Reset the Sync Account password

1. Open an InPrivate or Incognito browser.
2. Surf to portal.azure.com.
3. Logon as `AzureADSync@<yourdomainname>.onmicrosoft.com` with a password of `Temporary.Password`.
4. Update your password to `Complex.Password`.
5. Close the InPrivate or Incognito browser.

## Exercise 3 - Install Azure Active Directory Connect

1. Connect to the ADConnect VM and logon as `<yourADdomainname>\ADAdmin`, not `ADConnectAdmin` which is a local account.
    > Ensure that you are not logging on as local account!
2. When **Server Manager** opens select **Local Server** and turn off **IE Enhanced Security Configuration** for Administrators.
3. Open Internet Explorer, accept the defaults, and surf to [Microsoft Azure Active Directory Connect](http://go.microsoft.com/fwlink/?LinkId=615771).
4. Click **Download**, then **Run** when prompted.
Close Internet Explorer within the AD Connect virtual machine.

## Exercise 4 -  Configure Azure Active Directory Connect

1. Switch to the **Microsoft Azure Active Directory COnnect** window and on the **Welcome to Azure AD Connect** screen select **I agree** then **Continue**.
2. Review the screen and select **Use express settings**.
3. On the **Connect to Azure AD** screen enter your **Azure AD Credentials**:
    > Copy the credentials from your scratch pad.
   * USERNAME: `AzureADSync@<yourAzureADDomain>.onmicrosoft.com`
   * PASSWORD: `Complex.Password`
   * Click **Next** and then confirm the credential are validated.  Correct any errors.
4. On the **Connect to AD DS screen**, enter the Active Directory Domain Services domain administrator credentials:
    > Copy the credentials from your scratch pad.
   * USERNAME: `<yourADDomain>\adadmin`
   * PASSWORD: `Complex.Password`
   * Click **Next** and then confirm the credential are validated.
5. On the **Azure AD sign-in configuration** screen, select the checkbox for **Continue without any verified domains** and click **Next**.

   > Since this is a temporary lab environment we are not going use a validated custom domain.
6. On the **Ready to Configure** screen click **Install**.
7. It may take 5-10 minutes for Azure AD Connect to complete installation. Read the **Configuration Complete** screen and then click **Exit**.
8. Minimize your RDP window.

## Exercise 5 - Validate Synchronization

1. Switch to the Azure portal and examine your Azure AD Directory by selecting the xxxx.onmicrosoft.com  Directory from the upper right hand corner of the portal.
2. Under **Manage** select **Users**. Note that you should now see accounts sourced from Windows Server AD that have synchronized to Azure Active Directory (e.g. Bob & Julia).

> Congratulations!  Your are now synchronizing Active Directory to Azure Active Directory!!

### Continue with Lab 4: [Grant consent for WVD service](Prepare-Lab04-Grant-consent-for-WVD-service.md)

### Return to [Prepare Phase Labs](prepare.md)
