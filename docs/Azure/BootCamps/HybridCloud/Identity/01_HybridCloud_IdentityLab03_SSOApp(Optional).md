# Hybrid Identity Hands-On Lab

## Password single sign-on for an Azure AD gallery application

## Overview

In this lab you are going to publish an application (Facebook) from the Azure AD Application Gallery to select users and configure password-based single sign-on. Azure AD will securely store and replay the  usernames and passwords for Facebook.

## Before you Begin

If you are using a Microsoft Azure subscription that was provided to you by Microsoft, you are limited to a specific set of Microsoft Azure regions that you can use. Please use either the **East US, South Central US, West Europe, Southeast Asia, West US 2, or West Central US locations**. Otherwise you will receive an  error in the portal if you select an unsupported region and attempt to build anything in Microsoft Azure.

## Exercise 1 - Add some test users

We need to create some users in order to learn about application provisioning to users and groups.

1. From the Azure Portal, Click **Azure Active Directory**.  Ensure that you **DO NOT** have your default directory selected but rather the directory that has *.onmicrosoft.com in the namespace.
2. Click **Groups**, then **+New Group**, and enter the following:
    * Group Name: Facebook Authorized
    * Group Description: Users allowed to have SSO with Facebook
    * Click **Create**
3. Click your **Azure Active Directory**, **Users**, and then **+New User**, and enter the following:
    * User name: NoFace
    * Name: FacebookNo
    * Password: Select **Let me create the password** and enter `Temp.Password`
    * Click **Create**
4. Click  **+New User** and enter the following:
    * User name: YesFace
    * Name: FacebookYes
    * Password: Select **Let me create the password** and enter `Temp.Password`
    * Under Groups click **0 groups selected**.  Highlight and select  **Facebook Authorized** and then click **Select**.
    * Click **Create**

## Exercise 2 - Add Facebook from the Azure AD gallery

To add Facebook from the Azure AD gallery, follow these steps:

1. Click your **Azure Active Directory**, **Enterprise Applications**, **+New  Application**, and enter the following:
    * Enter **Facebook** in the **Add from the gallery** search box.
    * Select **Facebook** and then select **Add** to add the application. After a short period, you can see the application’s configuration pane.

## Exercise 3 - Configure Facebook for password single sign-on

To configure single sign-on for Facebook, follow these steps:

1. Select **Single sign-on** from the left menu under **Manage**.
2. Click on **Password-based** mode and then click **Save**.

## Exercise 4 - Assign an application to a group directly

In order to assign an application to a group we need change the version of Azure AD from the free version to Premium P2.

### Change Azure AD Version

1. Click on your **Azure Active Directory** in the Azure Portal.
2. Select **Getting started** and then **Get a free trial for Azure AD Premium**.
3. Expand **Free trial** under **AZURE AD PREMIUM P2** and then click **Activate**.
4. Select **Overview** and then refresh your browser,  Ensure that your Azure AD version reports as **Azure AD Premium P2** before continuing.  It may take 5-8 minutes for the upgrade to go live.

### Assign Groups

To assign one or more groups to an application directly, follow these steps:

1. Select **Enterprise Applications** under **Manage** and then select **Facebook**.
2. Click on **Users and Groups** from the menu on the left and select the **+Add user** button at the top of the pane.
3. If you see the message ***Groups are not available for assignment due to your Active Directory plan level*** then refresh your browser in 3-5 minutes and try again.
4. Select **Users and groups**  from the Add Assignment pane and then select **Facebook Authorized**.
5. When you're finished selecting groups, use the **Select** button to add them to the list of users and groups to be assigned to the application.
6. Select **Assign** to assign the application to the selected groups. After a short period, the users you've selected should be able to start these applications from the Access Panel.

## Exercise 5 - Test Access to Facebook

You are now going to logon as FacebookYes to configure your SSO credentials to validate functionality.  You will then logon as FacebookNo to see the difference.

### Confirm SSO Functionality

1. Open a web browser with an InPrivate or InCognito tab and go to <https://myapps.microsoft.com>, the My Apps portal.
2. Logon as FacebookYes (yesface@XXXX.onmicrosoft.com).  You will be prompted to change your temporary password.  Use `Complex.Password`.
3. Upon logon you should see Facebook listed.  Click on **Facebook**.
4. Install the MyApps Secure Sign-in Extension.  This extension helps you start any of your organization's cloud apps that require you to use a single sign-on process.  **Run** the extension when prompted and complete a default installation.
5. Click **Facebook** again and enter your Facebook credentials.  Azure AD will securely store and replay your username and password for Facebook moving forward.  This is a one-time-only entry of your credentials.  Click **Sign In** when done.
6. Close and then re-open a web browser with an InPrivate or InCognito tab and go to <https://myapps.microsoft.com>.  
7. Click **Facebook** and validate your logon if necessary.  Notice that you are taken directly to Facebook without having to enter your Facebook credentials.
8. Close the Facebook tab and on the MyApps portal page logout.

### Validate Application Restrictions

1. Logon to <https://myapps.microsoft.com>, the My Apps portal, as FacebookNo (noface@XXXX.onmicrosoft.com).
2. You will be prompted to change your temporary password.  Use *Complex.Password*.
3. Notice that Facebook is not presented to the user.
