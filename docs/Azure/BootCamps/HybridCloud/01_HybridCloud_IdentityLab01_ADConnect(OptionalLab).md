# Hybrid Identity Hands-On Lab

## Pass Thru Authentication and High Availability

## Before you Begin

If you are using a Microsoft Azure subscription that was provided to you by Microsoft, you are limited to a specific set of Microsoft Azure regions that you can use. Please use either the **East US, South Central US, West Europe, Southeast Asia, West US 2, or West Central US locations**.
Otherwise you will receive an  error in the portal if you select an unsupported region and attempt to build anything in Microsoft Azure.

## Step 2: Enable the feature

### Enable Pass-through Authentication through Azure AD Connect.

1. Select the Change user sign-in task on Azure AD Connect, and then select Next. 
2. Select Pass-through Authentication as the sign-in method. On successful completion, a Pass-through Authentication Agent is installed on the same server as Azure AD Connect and the feature is enabled on your tenant.

 ### Important
Pass-through Authentication is a tenant-level feature. Turning it on affects the sign-in for users across all the managed domains in your tenant.

## Step 3: Test the feature

Follow these instructions to verify that you have enabled Pass-through Authentication correctly:

1. Sign in to the Azure Active Directory admin center with the global administrator credentials for your tenant.
2. Select Azure Active Directory in the left pane.
3. Select Azure AD Connect.
4. Verify that the Pass-through authentication feature appears as Enabled.
5. Select Pass-through authentication. The Pass-through authentication pane lists the servers where your Authentication Agents are installed.

## Step 4: Ensure high availability

If you plan to deploy Pass-through Authentication in a production environment, you should install additional standalone Authentication Agents. Install these Authentication Agent(s) on server(s) other than the one running Azure AD Connect. This setup provides you with high availability for user sign-in requests.

### Important

In production environments, we recommend that you have a minimum of 3 Authentication Agents running on your tenant. There is a system limit of 40 Authentication Agents per tenant. And as best practice, treat all servers running Authentication Agents as Tier 0 systems (see reference).
Installing multiple Pass-through Authentication Agents ensures high availability, but not deterministic load balancing between the Authentication Agents. To determine how many Authentication Agents you need for your tenant, consider the peak and average load of sign-in requests that you expect to see on your tenant. As a benchmark, a single Authentication Agent can handle 300 to 400 authentications per second on a standard 4-core CPU, 16-GB RAM server.
To estimate network traffic, use the following sizing guidance:
Each request has a payload size of (0.5K + 1K * num_of_agents) bytes; i.e., data from Azure AD to the Authentication Agent. Here, "num_of_agents" indicates the number of Authentication Agents registered on your tenant.
Each response has a payload size of 1K bytes; i.e., data from the Authentication Agent to Azure AD.
For most customers, three Authentication Agents in total are sufficient for high availability and capacity. You should install Authentication Agents close to your domain controllers to improve sign-in latency.
To begin, follow these instructions to download the Authentication Agent software:
To download the latest version of the Authentication Agent (version 1.5.193.0 or later), sign in to the Azure Active Directory admin center with your tenant's global administrator credentials.
Select Azure Active Directory in the left pane.
Select Azure AD Connect, select Pass-through authentication, and then select Download Agent.
Select the Accept terms & download button.