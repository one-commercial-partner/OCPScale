# Lab 9: Test Your WVD Deployment

In this lab your will validate the personal and Pooled Windows Virtual Desktop pools that you created by connecting to them via the Windows Desktop Client and the Web Client.

## Exercise 1 - Connect with the Windows Desktop client

1. Choose the client that matches your version of Windows:
    * [Windows 64-bit](https://go.microsoft.com/fwlink/?linkid=2068602)
    * [Windows 32-bit](https://go.microsoft.com/fwlink/?linkid=2098960)
    * [Windows ARM64](https://go.microsoft.com/fwlink/?linkid=2098961)
2. Complete a default installation and select the box for **Launch Remote Desktop when setup exits.**
3. In the Remote Desktop client, click on **Subscribe**.
4. When prompted enter the credentials for Bob Jones:
    * Username: `Bob.Jones@yourdomain.onmicrosoft.com`
    * Password: `Complex.Password`
5. Click **Yes** on the next screen and the **Done** on the following screen.
6. Double-click on the icon for Personal Pool icon.
7. When prompted enter the credentials for Bob Jones:
    * Username: `Bob.Jones@<yourADdomain>`
    * Password: `Complex.Password`
8. Once connected, change the desktop background to a different picture.
9. Open a command prompt and take note of the IP address for a future exercise.
10. Disconnect from the session.

## Exercise 2 - Connect with the Windows Virtual Desktop web client

1. In a InPrivate or InCognito browser, navigate to the [Windows Virtual Desktop web client](https://rdweb.wvd.microsoft.com/webclient).
2. When prompted enter the credentials for Julia Williams by clicking on **+ Use another account**:
    * Username: `Julia.Williams@<yourAzureADdomain>.onmicrosoft.com`
    * Password: `Complex.Password`
    > **NOTE:** These credentials are to logon and authenticate to the Windows Virtual Desktop Service.
3. Click on the icon for PooledPool.
4. Click **Allow** on the **Access local resources** window.
5. When prompted enter the following credentials:
    * Username: `yourADdomain`\ Julia.Williams
    * Password: `Complex.Password`
    > **NOTE:** These credentials are to logon to a computer in the pool.  
6. Once connected, change the desktop background to a different picture.
7. Disconnect from the session.

## Continue to [FSLogix Installation and Configuration with File Shares](../Optimize/Optimize-FSLogix-Installation-and-Configuration)

## Back to [Technical Intensity Workshop - Windows Virtual Desktop Labs](../index.md)
