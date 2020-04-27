# Lab 1: Prerequisites

In the lab you will complete all the necessary prerequisites to build out a Windows Virtual Desktop environment.

## Your enviroment

We recommend having multiple monitors to complete this lab.  On one monitor load your web browser to the Azure Portal and on the other have this lab guide visible.

## Azure subscription

Your will need an Azure subscription to complete the Windows Virtual Desktop labs.  The following are ideal subscription types to utilize:

* Visual Studio Professional or Visual Studio Enterprise
* IUR (Azure Use Rights) from your partner organization

**Microsoft does not recommend using any Azure subscription that has production workloads or services.  Use a subscription that is designated for testing purposes only.**

If you are using IUR, contact the subscription administrator at your partner organization.  The can provision your identity into the subscription and assign the appropriate rights.

> Please ensure that you are using a **clean** subscription, meaning your subscription has no connections to any existing subscriptions, identities, accounts, etc.

You will consume roughly $25.00 in your subscription configuring this lab.

In order to complete the **Prepare** and **Deploy** phase labs, you will need 16 vCPUs available in your subscription.  

Each lab in the **Optimize** phase is  relatively lengthy in duration and may require additional resouces within your Azure subscription such as storage accounts, vCPUS, etc..

## Rights assignments

Your identity within your subscription must have the following rights assignments:

* **Global administrator** permissions within the Azure Active Directory tenant.
  * This also applies to Cloud Solution Provider (CSP) organizations that are creating a Windows Virtual Desktop tenant. If you're in a CSP organization, you must be able to sign in as global administrator.
  * The administrator account must be sourced from the Azure Active Directory tenant in which you're trying to create the Windows Virtual Desktop tenant. This process doesn't support Azure Active Directory B2B (guest) accounts.
  * The administrator account must be a work or school account
* Ensure that the user who will provision & configure WVD must have at least **Contributor** rights to the Azure subscription.
  * Based on the operating model, some customers might not have this enabled so contact your CSP-Partner who can help with the same.

## Exercise 1 - Install Windows Virtual Desktop PowerShell Module

Complete these steps to install the Windows Virtual Desktop PowerShell module on your local computer:

1. To quickly download and install the Windows Virtual Desktop PowerShell module, launch PowerShell as an administrator and run the following command:

    ```PowerShell
    Install-Module -Name Microsoft.RDInfra.RDPowershell
    ```

    *Type **Y** when prompted for installing from an untrusted repository.*

## Exercise 2 - Capture your Azure Subscription ID

1. Open the [Azure Portal](portal.azure.com) and logon.

2. Click on **Microsoft Azure** and then **Subscriptions**.

3. Click on your subscription, and then on the main pne click on the Subscription ID: line, and then click on the icon to copy the information.

4. Paste the information in a scratch location such as Notepad.

### Continue with Lab 2: [Deploying Azure Infrastructure and AD-DS](Prepare-Lab02-Azure-infrastructure.md)

### Return to [Prepare Phase Labs](prepare.md)
