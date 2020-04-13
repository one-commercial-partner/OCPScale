# Lab 1: Deploying an Azure AD Tenant

In the lab you will complete all the necessary prerequisites to build out a Windows Virtual Desktop environment.

## Azure subscription

Your will need an Azure subscription to complete the Windows Virtual Desktop labs.  The following are ideal subscription types to utilize:

* Visual Studio subscription
* IUR (Azure Use RIghts) from your partner organization

**Microsoft does not recommend using any Azure subscription that has production workloads or services.  Use a subscriptin that is designation for testing purposes only.**

If you are using IUR, contact the subscription administrator at your partner organization.  The can provision your identity into the subscription and assign the appropriate rights.

## Rights assignments

Your identity within your subscription must have the following rights assignments:

* **Global administrator** permissions within the Azure Active Directory tenant.
  * This also applies to Cloud Solution Provider (CSP) organizations that are creating a Windows Virtual Desktop tenant. If you're in a CSP organization, you must be able to sign in as global administrator.
  * The administrator account must be sourced from the Azure Active Directory tenant in which you're trying to create the Windows Virtual Desktop tenant. This process doesn't support Azure Active Directory B2B (guest) accounts.
  * The administrator account must be a work or school account
* Ensure that the user who will provision & configure WVD must have at least **Contributor** rights to the Azure subscription.
  * Based on the operating model, some customers might not have this enabled so contact your CSP-Partner who can help with the same.

## Install Windows Virtual Desktop PowerShell Module

Complete these steps to install the Windows Virtual Desktop PowerShell module:

1. To quickly download and install the Windows Virtual Desktop PowerShell module, launch PowerShell as an administrator and run the following command:

    `Install-Module -Name Microsoft.RDInfra.RDPowershell`

    *Type **Y** when prompted for installing from an untrusted repository.*

## Install Azure Active Directory

1. In the Azure Portal, click **Microsoft Azure** and then **+Create a resource**.  Select **Identity** and then **Azure Active Directory**.
2. Enter the following on the **Create directory tab**:
    * Organization name: *yourfirstname* Organization (e.g. Mike's Organization)
    * Initial domain name: WVD*yourinitials*Lab (e.g. WVDXYZLab).  Hit **Tab**.

        *Ensure validation passes as your namespace needs to be unique within the onmicrosoft.com namespace.  We often see students choosing a domain name that already exists.*

        ***Write this domain name down as your Azure Active Directory Domain Name.***

        ![AzureADSetup](../attachments/AzureADSetup.PNG)

3. Click **Create**.  It will take several minutes for the directory to be created.
4. Once complete, select Click **here** to manage your new directory.

Return to [Prepare Phase Labs](prepare.md)
