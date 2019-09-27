# Traffic Manager
This lab requires that you have deploy two instances of a web application running in different Azure regions (East and West US). The two web application instances serve as primary and backup endpoints for Traffic Manager.

## Task 1 - Create East US Web App
1.	On the top left-hand side of the Azure Portal select **+Create a resource** > **Web** > **Web App**. 
2.	In Web App, enter or select the following information and enter default settings where none are specified:
    * App name: `yourinitals`EastWebApp (e.g. abcEastWebApp)
    * Resource Group: *Create New* **EastWebApps**
    * Click on App Service\Locations and then:
        * Create New: **EastWebApps**
        * App Service plan name: `yourinitals`EastAppSvcPlan
        * Location: **East US**
        * Pricing Tier: **D1 Shared**
        * Click **OK**
3)	Select **Create**.  A default website is created when the Web App is successfully deployed.

## Task 2 - Create West US Web App
1.	On the top left-hand side of the Azure Portal select **+Create a resource** > **Web** > **Web App**.
2.	In Web App, enter or select the following information and enter default settings where none are specified:
    * App name: `yourinitals`WestWebApp (e.g. abcEastWebApp)
    * Resource Group: *Create New* **WestWebApps**
    * Click on App Service\Locations and then:
        * Create New: **WestWebApps**
        * App Service plan name: `yourinitals`WestAppSvcPlan
        * Location: **West US**
        * Pricing Tier: **D1 Shared**
        * Click **OK**
3)	Select **Create**.  A default website is created when the Web App is successfully deployed.

## Task 3 - Create a Traffic Manager profile
Create a Traffic manager profile that directs user traffic based on endpoint priority.
1.	On the top left-hand side of the screen, select **+Create a resource** > **Networking** > **Traffic Manager profile**.  You may have to type *Traffic* into the search bar and click **Create**.
2.	In the Create Traffic Manager profile, enter or select, the following information, accept the defaults for the remaining settings, and then select **Create**:
    * Name: <yourinitials>TM (This name needs to be unique within the trafficmanager.net zone and results in the DNS name, trafficmanager.net which is used to access your Traffic Manager profile.)
    * Routing method: Priority
    * Resource Group: EastWebApps

## Task 4 - Add Traffic Manager endpoints
Add the website in the East US as primary endpoint to route all the user traffic. Add the website in West Europe as a backup endpoint. When the primary endpoint is unavailable, traffic is automatically routed to the secondary endpoint.
1.	In the portal’s search bar, search for the Traffic Manager profile name that you created in the preceding section and select the profile in the results that the displayed.  You can also click on **All resources**.
2.	In Traffic Manager profile, in the Settings section, click **Endpoints**, and then click **+Add**.
3.	Enter, or select, the following information, accept the defaults for the remaining settings, and then select **OK**:
    * Type: Azure endpoint
    * Name: **PrimaryEndpoint**
    * Target resource type: App Service
    * Target Resource: <yourinitals>EastWebApp
    * Priority: 1
4. In Traffic Manager profile, in the Settings section, click **Endpoints**, and then click **+Add**.
5.	Enter, or select, the following information, accept the defaults for the remaining settings, and then select **OK**:
    * Type: Azure endpoint
    * Name: **SecondaryEndpoint**
    * Target resource type: App Service
    * Target Resource: <yourinitals>WestWebApp
    * Priority: 2
 

## Task 5 - Test Traffic Manager profile
In order for Traffic Manager to function properly, a FQDN domain name is required as Traffic Manager works on the DNS level.  For this lab we do not have a public domain name available that we can use, so your results in testing are not representative of what you would normally see when you have a proper configuration.

If you have a custom domain name available, please complete the steps in this tutorial, and then the steps listed below.

[Configuring a custom domain name for a web app in Azure App Service using Traffic Manager](https://docs.microsoft.com/en-us/azure/app-service/web-sites-traffic-manager-custom-domain-name)


1.	Click **Overview** on the current blade and the Traffic Manager profile displays the DNS name of your newly created Traffic Manager profile. Copy that information.
2.	In a web browser, paste the DNS name of your Traffic Manager profile to view your Web App's default website. In this lab scenario, all requests are routed to the primary endpoint that is set to Priority 1.

**Because we do not have a custom domain name configured you will receive a 404 Web Site not found error.**

3.	The following step only works if you have a custom domain name configured. To view Traffic Manager failover in action, disable your primary site as follows:
    * In the Traffic Manager Profile page, select Settings>Endpoints>MyPrimaryEndpoint.
    * In MyPrimaryEndpoint, select Disabled.
    * The primary endpoint MyPrimaryEndpoint status now shows as Disabled.
3)	Copy the DNS name of your Traffic Manager Profile from the preceding step to successfully view the website in a web browser. When the primary endpoint is disabled, the user traffic gets routed to the secondary endpoint.

 
