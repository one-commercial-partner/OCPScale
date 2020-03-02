WVD Lab
3 | P a g e ©2019 Microsoft Corporation
Terminology
WVD – Windows Virtual Desktop
GA – Azure Active Directory Global Administrator
ARM – Azure Resource Manager
Sub – Subscription
AAD – Azure Active Directory “ilovewvd.onmicrosoft.com”
AD – Active Directory “ilovewvd.com”
Tenant – Azure AD Tenant Name “ilovewvd.onmicrosoft.com”
Custom Domain – Your purchased domain from a registrar such as Go-Daddy “ilovewvd.com”
AAD Domain – Azure Active Directory “ilovewvd.onmicrosoft.com”
Domain Admin – custom domain \ AD account that has rights to add servers to your domain etc.. See below
Accounts needed for deployment of the entire WVD solution
Azure AD Global Admin
• Grant Azure Active Directory permissions to the Windows Virtual Desktop service
• Assign the Tenant Creator Application role to a user in your Azure Active Directory
• submit WVD applications (there are two server and client)
• Consent to WVD applications
Active Directory Domain/Account Admin
• Domain Join WVD Session Hosts
• Needed to create security groups
• Needed to create test users
• Needed to add production users to security groups – although this is irrelevant at this point as WVD does not support groups in anyway – this is on the roadmap
WVD Tenant Creator
• This is an AAD account that DOES NOT have MFA enabled that will create the WVD resources in a subscription. This account should have contributor role access to the subscription hosting the WVD resources.
• Creates WVD tenant
• Creates WVD host pools
• Creates WVD Application Groups – These last two tasks could also be done by another account that has RDS permissions but is also NOT a tenant creator – and is arguably best practices (New-RdsRoleAssignment -RoleDefinitionName "RDS Owner")
WVD RDS Owner
• Creates WVD tenant
• Creates WVD host pools
• Creates WVD Application Groups
• Assigns Users
• All WVD Administrative duties