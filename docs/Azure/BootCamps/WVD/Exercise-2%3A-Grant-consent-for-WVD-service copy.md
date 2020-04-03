Exercise 2: Grant consent for WVD service 
------------------------------------------
---
Creating a tenant in Windows Virtual Desktop is the first step toward building your desktop virtualization solution. A (WVD) tenant is a group of one or more host pools. With a (WVD) tenant, you can build host pools, create app groups, assign users, and make connections through the service. 

Before you can create a Windows Virtual Desktop tenant, you must allow Windows Virtual Desktop services to access your Azure AD tenant. The way Windows Virtual Desktop is designed requires explicit Azure AD consent. The process is much like how Azure requires you to enable non-standard resource providers before being able to use them.

---

###Enabling WVD consent to Azure AD 


1. Browse to the URL: [https://rdweb.wvd.microsoft.com.](https://rdweb.wvd.microsoft.com/) Login with the Global Admin Account for your new AAD tenant. 
1. There are two steps that need to take place. First select from the dropdown, **server** and then paste your AAD ID in the field box.  

![image.png](/.attachments/image-42cd5bed-6229-4fb4-a148-1995ef508685.png)

1. Click **Accept** on the permissions request page                                                                                                     
1. *WAIT* One Minute before moving to next step. It can take about a minute for the server registration to complete. If you proceed to the next step too quickly, you may see an error.                                                                                                         
1. Navigate back to [https://rdweb.wvd.microsoft.com ](https://rdweb.wvd.microsoft.com/)  
1. Next, From the drop down select **Client App** from the drop-down box and add the same AAD ID again then **submit**.
   
![image.png](/.attachments/image-435889b9-272d-42af-933b-56302d870b35.png)  
                                                                                                                                                                                                                       
1. Login with the same Azure AD Global Admin Account you created earlier 
1. Click **Accept** on the permissions request page. Once this is complete you can close out of your browser session.                                   



