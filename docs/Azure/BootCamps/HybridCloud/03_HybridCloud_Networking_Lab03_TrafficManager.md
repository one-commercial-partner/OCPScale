# Lab 3 - Traffic Manager

In this lab you will deploy two instances of a web application running in two different Azure regions supported by your subscription. The two web application instances serve as primary and backup endpoints for Traffic Manager.

## Create US Web App

1. On the top left-hand side of the screen, select **Create a resource** > **Web** > **Web App**
2. In Web App, enter or select the following information and enter default settings where none are specified:
    * Resource Group: (create new)  **`<yourinitals>`-USWebApps** (e.g. abc-USWebApps)
    * App name: **`<yourinitals>`-USWebApp** (e.g. abc-USWebApp)
    * Runtime stack: **.NET Core 3.0 (Current)**
    * Region: **East US**
    * Sku and size: Change size to **Dev/Test D1**
    * Click **Apply**
3. Select **Review + create** and then **Create**.  A default website is created when the Web App is successfully deployed.

## Create Europe Web App

1. On the top left-hand side of the screen, select **Create a resource** > **Web** > **Web App**
2. In Web App, enter or select the following information and enter default settings where none are specified:
    * Resource Group: (create new)  **`<yourinitals>`-EUWebApps** (e.g. abc-EUWebApp)
    * App name: **`<yourinitals>`EUWebApp** (e.g. abc-EUWebApp)
    * Runtime stack: **.NET Core 3.0 (Current)**
    * Region: **West Europe**
    * Sku and size: Change size to **Dev/Test D1**
    * Click **Apply**
3. Select **Review + create** and then **Create**. A default website is created when the Web App is successfully deployed.

## Create a Traffic Manager profile

Azure Traffic Manager helps reduce downtime and improve responsiveness of important applications by routing incoming traffic across multiple deployments in different regions. Built-in health checks and automatic re-routing help ensure high availability if a service fails. Use Traffic Manager with Azure services including Web Apps, Cloud Services and Virtual Machines - or combine it with on-premises services for hybrid deployments and smooth cloud migration.

1. On the top left-hand side of the screen, select **Create a resource** > **Networking** > **Traffic Manager profile**. You may have to type in Traffic Manager Profile.
2. In the Create Traffic Manager profile, enter or select the following information and accept the defaults for the remaining settings:
    * Click **Create**
        * Name: `<yourinitials>`-TM (This name needs to be unique within the trafficmanager.net zone.)
        * Routing method: Geographic
        * Resource Group: `<yourinitials>`-USWebApps
    * Click **Create**

## Add Traffic Manager endpoints

Add the website in the East US as primary endpoint to route all the user traffic. Add the website in West Europe as a backup endpoint. When the primary endpoint is unavailable, traffic is automatically routed to the secondary endpoint.

1. In the portal’s search bar, search for the Traffic Manager profile name that you created in the preceding section and select the profile in the results that the displayed.  You can also go to the resource from the Alerts window.
2. In Traffic Manager profile, in the **Settings** section, click **Endpoints**, and then click **+Add**.
3. Enter the following information:
    * Type: **Azure endpoint**
    * Name: **USEndPoint**
    * Target resource type: **App Service**
    * Target Resource: `<yourinitals>`**-USWebApp**
    * Geo-Mapping:
        * Regional Grouping: **North America / Central America / Caribbean**
        * Country/Region: **United States**
        * State/Province:  **New York**
    * Add a second mapping :
        * Regional Grouping: **North America / Central America / Caribbean**
        * Country/Region: **United States**
        * State/Province:  **Massachusetts**
    * Select **OK**
4. Click **+Add** and enter the following information:
    * Type: **Azure endpoint**
    * Name: **EUEndPoint**
    * Target resource type: **App Service**
    * Target Resource: `<yourinitals>`-EUWebApp
    * Geo-Mapping:
        * Regional Grouping: **Europe**
        * Country/Region: **United Kingdom**
    * Select **OK**
5. Before proceeding confirm that the status of both endpoints is **Online**.

## Test Traffic Manager profile

In this section, you first determine the domain name of your Traffic Manager profile and then view how Traffic Manager fails over to the secondary endpoint when the primary endpoint is unavailable.

Since we have enabled traffic management on a global versus regional perspective, we need to test access to your website from various locations around the world.

### Determine the DNS name

1. Click **Overview** and the Traffic Manager profile displays the DNS name of your newly created Traffic Manager profile.
2. Copy the URL to the clipboard.

### View Traffic Manager in action

1. In a web browser, surf to <https://www.whatsmydns.net> and enter the DNS name of your Traffic Manager profile, change the record type to CNAME, and click **Search**.
2. Notice which of your endpoints are providing services to the various global locations.
3. In the Azure portal swith to your Traffic Manager profile and notice that all of your endpoints are **Enabled**.
4. Click on **USEndpoint**, select **Disabled**, the **Save**.
5. In the Azure portal switch to your Traffic Manager profile and notice that **MyPrimaryEndpoint** is now **Disabled**.
6. Return to <https://www.whatsmydns.net> and click **Search**, noticing the changes on which endpoint(s) are now responding.
7. Note that it may take several moments for DNS propagation to take place.
