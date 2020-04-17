# Lab 7: Deploy a Personal Host Pool

Now that we have provisioned a Windows Virtual Desktop Tenant, we can now deploy a Personal Host Pool to publish resources to our users.

Your Windows Virtual Desktop tenant is the management space for Host Pools, one of the host pool types is Personal Hosts. This means that the Host will be statically assigned to a single user or multiple users. In this situation the user logs into the same host every single time. We treat Persistent host much the same way we do traditional workstations, since they are typically running 24/7, they need to be patched and maintained like a typical workstation.

There are many ways to deploy a Personal Host Pool however we will focus on leveraging the Azure Marketplace. For more advanced deployments you can use ARM templates as is documented here: [Tutorial: Create a host pool by using the Azure Marketplace](https://docs.microsoft.com/en-us/azure/virtual-desktop/create-host-pools-azure-marketplace)

## Exercise 1 - Provision a Personal Host Pool

1. Return to the [Azure Portal](https://portal.azure.com) and search for **Marketplace**.  
    > **NOTE:** Ensure that you are in the correct directory and subscription.

    ![image](../attachments/4e91cf3c29be44f486c9b7428235071c.png)

2. While in the **Marketplace** search for **Windows Virtual Desktop - Provision a host pool**

    ![image](../attachments/8be16b1ed7e18681ce7554cf8c13bf57.png)

3. Select the **Windows Virtual Desktop - Provision a host pool** and then click the **Create** button to provision this service.

    ![WVDProvisionHostPool](../attachments/WVDProvisionHostPool.PNG)

4. Complete the **Basics** tab with the following information:
    * Resource Group: **WVDLab**
    * Region: **Choose the same region where you placed previous resources**
    * Hostpool name: **Personal**
    * Desktop type: **Personal**

5. Complete the **Configure virtual machines** tab with the following information:
    * Total users: **2**
        >This will create 2 hosts and join them to AD and this pool.
    * Virtual machine size: *Change Size* and select **B2s**
        >This size is fine for lab purposes but you would choose larger VMs for production.  Based upon your previous lab experience choose the smallest VM size that is available to you.
    * Virtual machine name prefix: **WVDPers**
        >This prefix will be used in combination with the VM number to create the VM name. If using 'WVDPers' as the prefix, VMs would be named 'WVDPers-0', 'WVDPers-1', etc. You should use a unique prefix to reduce name collisions in Active Directory and in Windows Virtual Desktop.

6. Complete the **Virtual machine settings** tab with the following information:
    * AD domain join UPN: `WVDAdmin@<yourADDomain>`
        >UPN of an Active Directory user that has permissions and will be used to join the virtual machines to your domain.  If you didn't write this down you can return to your RDP session with the domain controller and obtain the information.
    * Admin Password: `Complex.Password`
    * Confirm password: `Complex.Password`
    * Virtual network: **Select the existing virtual network created earlier, do not create a new VNET**
    * vmSubnet: **Select the existing subnet you created earlier, do not create a new subnet**

7. Complete the **Windows Virtual Desktop information** tab with the following information:
    * Windows Virtual Desktop tenant group name
        >**DO NOT CHANGE THIS NAME!**
    * Windows Virtual Desktop tenant name:  `<yourTenantName>`
        >Provide the tenant name used earlier. If the name does not exactly match your deployment will fail.  If your PowerShell window is still open you should be able to retrieve the name there.
    * UPN: `WVDAdmin@<yourAzureADDomain` 
    * Password: `Complex.Password`
    * Confirm password: `Complex.Password`

8. Select **Review + Create**. Wait for a **Validation Passed** and if you get a failure examine the **Activity log** in the Azure portal and resolve the failure.

   ![ValidationFailed](../attachments/ValidationFailed.PNG)

9. Select **Create** to start your Host Pool Deployment.

10. You can watch the progress of the deployment.  Note that this will take about 15 minutes or so to complete, so it might be a good time to stretch your legs and take a break.

    ![image](../attachments/763dbbfd0796fd7afecf51de9562d959.png)
11. You should eventually receive a message **“Your Deployment is complete”.** If
you receive a failure message refer to the step it failed at and refer to the
troubleshooting section on this guide.

    ![image](../attachments/d186f32593dbd7d350ec18940f547f8f.png)

Congrats! You have successfully deployed a Personal Host Pool!

## Exercise 2 - Assign users to the Pool

Now use the following commands to ensure your users are a member of the Personal Host pool.

This cmd will ensure the user is a member of the Application Pool, this is required to have access to see the session host.

1. In the Azure Portal open the Azure Active DIrectory for your tenant.
2. Select **Users** then **Bob Jones**.
3. Copy the UPN: `Bob.Jones@<yourdomain>.onmicrosoft.com`
4. Open PowerShell and enter the following command:

    ```Powershell
    Add-RdsAppGroupUser <enteryourtenantname> -HostPoolName PersonalPool -AppGroupName "Desktop Application Group" -UserPrincipalName Bob.Jones@<yourdomain>.onmicrosoft.com
    ```

### Return to [Deploy Phase Labs](deploy.md)
