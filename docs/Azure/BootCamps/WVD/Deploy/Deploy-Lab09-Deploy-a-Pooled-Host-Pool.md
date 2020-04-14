# Lab 9: Deploy a Pooled Host Pool

Now that we have provisioned a Personal Host Pool, we can now deploy a Pooled Host Pool to publish resources to our users.

## Exercise 1 - Provision a Pooled Host Pool

1. Return to the [Azure Portal](https://portal.azure.com) and search for **Marketplace**.  
    > **NOTE:** Ensure that you are in the correct directory and subscription.

    ![image](../attachments/4e91cf3c29be44f486c9b7428235071c.png)

2. While in the **Marketplace** search for **Windows Virtual Desktop - Provision a host pool**

    ![image](../attachments/8be16b1ed7e18681ce7554cf8c13bf57.PNG)

3. Select the **Windows Virtual Desktop - Provision a host pool** and then click the **Create** button to provision this service.

    ![WVDProvisionHostPool](../attachments/WVDProvisionHostPool.png)

4. Complete the **Basics** tab with the following information:
    * Resource Group: **WVDLab**
    * Region: **Choose the same region where you placed previous resources**
    * Hostpool name: **PooledPool**
    * Desktop type: **Pooled**

5. Complete the **Configure virtual machines** tab with the following information:
    * Create an Availability Set: **No**
    * Usage Profile: **Custom**
        > Select a usage profile to determine the number of users per vCPU--Light (6), Medium (4), or Heavy (2)--to use to calculate the number of virtual machines to create based on the selected VM size. If you would like to create a specific number of VMs, select 'Custom'.
    * Total users: **2**
        >This will create 2 hosts and join them to AD and this pool.
    * Virtual machine size: *Change Size* and select **B2s**
        >This size is fine for lab purposes but you would choose larger VMs for production.
    * Virtual machine name prefix: **WVDPool**
        >This prefix will be used in combination with the VM number to create the VM name. If using 'WVDPool' as the prefix, VMs would be named 'WVDPool-0', 'WVDPool-1', etc. You should use a unique prefix to reduce name collisions in Active Directory and in Windows Virtual Desktop.

6. Complete the **Virtual machine settings** tab with the following information:
    * AD domain join UPN: **WVDAdmin@(your AD Domain Name)** (e.g. adadmin@wagswvd.com)
        >UPN of an Active Directory user that has permissions and will be used to join the virtual machines to your domain.  If you didn't write this down you can return to your RDP session with the domain controller and obtain the information.
    * Admin Password: `Complex.Password`
    * Confirm password: `Complex.Password`
    * Virtual network: **Select the existing virtual network you created earlier, do not create a new VNET**
    * vmSubnet: **Select the existing subnet you created earlier**

7. Complete the **Windows Virtual Desktop information** tab with the following information:
    * Windows Virtual Desktop tenant group name
        >**DO NOT CHANGE THIS NAME!**
    * Windows Virtual Desktop tenant name:  wvdXYZlabs.onmicrosoft.com
        >Provide the tenant name used earlier. If the name does not exactly match your deployment will fail.  If your PowerShell window is still open you should be able to retrieve the name there.
    * UPN: **WVDAdmin@(your Azure AD Domain Name)** (e.g. wvdadmin@domain.onmicrosoft.com)
    * Password: `Complex.Password`
    * Confirm password: `Complex.Password`

8. Select **Next: Review + Create**. Wait for a **Validation Passed,** and if you get a failure examine the **Activity log** in the Azure portal and resolve the failure.

   ![ValidationFailed](../attachments/ValidationFailed.png)

9. Select **Create** to start your Host Pool Deployment.

10. You can watch the progress of the deployment.  Note that this will take about 15 minutes or so to complete, so it might be a good time to stretch your legs and take a break.

    ![image](../attachments/763dbbfd0796fd7afecf51de9562d959.png)
11. You should eventually receive a message **“Your Deployment is complete”.**

    ![image](../attachments/d186f32593dbd7d350ec18940f547f8f.png)

Congrats! You have successfully deployed a Pooled Host Pool!  

## Exercise 2 - Assign users to the Pool

Now use the following commands to ensure your users are a member of the Pooled Host pool.

This cmd will ensure the user is a member of the Application Pool, this is required to have access to see the session host.

1. In the Azure Portal open the Azure Active DIrectory for your tenant.
2. Select **Users** then **Julia Williams**.
3. Copy the UPN: `Julia.Williams@<yourdomain>.onmicrosoft.com`
4. Open PowerShell and enter the following command:

    ```Powershell
    Add-RdsAppGroupUser <enteryourtenantname> PooledPool "Desktop Application Group" -UserPrincipalName Julia.Williams@<yourdomain>.onmicrosoft.com
    ```

## Exercise 3 - Connect with the Windows Virtual Desktop web client

1. In a browser, navigate to the [Windows Virtual Desktop web client](https://rdweb.wvd.microsoft.com/webclient).
2. When prompted enter the credentials for Julia Williams:
    * Username: Julia.Williams@`yourdomain`.onmicrosoft.com
    * Password: `Complex.Password`
    > **NOTE:** These credentials are to logon and authenticate to the Windows Virtual Desktop Service.
3. Click on the icon for PooledPool.
4. Click **Allow** on the **Access local resources** window.
5. When prompted enter the following credentials:
    * Username: wvdadmin@<yourADdomain>
    * Password: `Complex.Password`
    > **NOTE:** These credentials are to logon to a computer in the pool.
6. Once connected, change the desktop background to a different picture.
7. Disconnect from the session.

### Return to [Deploy Phase Labs](deploy.md)
