Lab 3: Deploying Azure Infrastructure and AD DS
--------------------------------------------------
---
In this exercise you will leverage a custom Azure Resource Manager (ARM) template to deploy the required Active Directory Domain Services infrastructure for WVD. If you already have an AD DS environment and AD Connect configured, you can move on to [Exercise 4: Configuring Azure AD Connect with AD DS](/Windows-Virtual-Desktop-on-Azure-Lab/Prerequisites/Exercise-4:-Configuring-Azure-AD-Connect-with-AD-DS).

Link to the ARM template: [AAD hybrid lab ARM template from GitHub](https://github.com/PeterR-msft/M365WVDWS/tree/master/AAD-Hybrid-Lab).

This ARM Template will provision the following resources:

- Virtual Network
   - 1 Subnet
   - 1 Network Security Group (NSG)
     - Permits AD traffic, permits RDP incoming traffic, restricts DMZ access.
   - DNS configured to point to the domain controller.
- Virtual Machine
   - Active Directory Domain Services is installed and configured.
   - Test users created in the domain.
   - Azure AD Connect is installed and ready for configuration.
   - Public IP address assigned for remote administration via RDP.

---
### Task 1: Deploying the ARM template 

> *Tip:* Internally at MSFT we have different tenant directories available. Because of this, it is not  
> uncommon for ARM templates hosted on GitHub to get stuck during deployment. To avoid confusion and 
> deployment errors, **open an InPrivate browser window** when deploying ARM templates from GitHub.

1. Open a browser and navigate to the ARM template: (https://github.com/PeterR-msft/M365WVDWS/tree/master/AAD-Hybrid-Lab).

   > Note: This ARM template was developed by Peter Riedelsdorf. Extra kudos to him for simplifying the lab!

2. Review the notes on the GitHub page for the ARM template prior to deployment.
3. Under Quick Start, click **Deploy to Azure**. This will open a new browser tab to the Azure Portal 
for custom deployments. 

   ![PreReqs-Ex03000.png](../attachments/PreReqs-Ex03000-c3ed867f-3d56-4759-96b6-7636aa557534.png)

4. If prompted, make sure to sign in with an account that is an owner for the Azure subscription.

   ![PreReqs-Ex03001.png](../attachments/PreReqs-Ex03001-a65ea79e-2a12-4fd2-80cf-946dec64e987.png)

5. Fill in the required ARM template parameters. Refer to the following example for more information on the parameters.

   ![PreReqs-Ex03002.png](../attachments/PreReqs-Ex03002-87965f80-eed9-4086-8ac0-6e68f0509725.png)

6. Agree to the Terms and conditions and click **Purchase**.  

   ![PreReqs-Ex03003.png](../attachments/PreReqs-Ex03003-1af935eb-5e34-43c7-9824-dc0d256940a7.png)
                      
The deployment is now underway. On average this process can take 30 minutes to complete. It is important 
that you monitor the deployment progress to ensure there are no problems. You can monitor progress by 
clicking the **notification** bell in the upper right corner and clicking **Deployment in progress...**.

![PreReqs-Ex03004.png](../attachments/PreReqs-Ex03004-ecd71059-001c-4554-ada1-e81929cca805.png)

> *Note:* While automation can make things simpler and repeatable, sometimes it can fail. If at any time during the ARM template deployment there is a failure, review the failure, delete the Resource Group and try the ARM template again, adjusting for any possible errors.                                                                       
                                                                                                                                                                                    
Once the ARM template is done being deployed, the status will change to complete. At this point the domain controller is ready for RDP connectivity. 

![PreReqs-Ex03005.png](../attachments/PreReqs-Ex03005-0cc771eb-6516-4f11-84d9-0c6549a9c254.png)
