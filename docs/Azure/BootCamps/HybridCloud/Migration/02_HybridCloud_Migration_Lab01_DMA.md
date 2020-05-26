# Database Migration Assistant (DMA)

In this lab you are going to use DMA to migrate a SQL server database over to Azure.

## Before you Begin

For this lab you have a choice to make:  use a pre-built SQL Server as a source or build your own.

If you use the **pre-built SQL server** as a source, you get directly to migration.  Take this choice if you are already familiar with SQL.  Start with `Exercise 6`.

If you choose **build your own SQL server** you get additonal hands-on experience with SQL.  Use this option if you are using a non-Windows computer as your local device. Start with `Exercise 1`.

There is no significant time saving between either option.

## Exercise 1 - Create a SQL Server VM image

1. Log in to the Azure portal using your account.
2. On the Azure portal, click **+Create a resource**.
3. In the search field, type **SQL Server 2017  on Windows Server 2019** and press **ENTER**.
4. Under **Select a software plan** choose select the **SQL Server 2017 Standard on Windows Server 2019** and click **Create**.
5. Enter the following:
    * Resource Group: *create new* **SQLMIG**
    * Virtual Machine Name: **SQLVM**
    * Region: Choose a supported region
    * Availability Options: No infrastructure redundancy required
    * Size: Select **D2s_v3**
    * Username: `yourfirstname`
    * Password: `Complex.Password`
    * Confirm Password: `Complex.Password`
    * Public inbound ports: **Allow selected ports**
    * Select inbound ports: **RDP (3389)**
6. Click **Next: Disks >**
7. Click on the **SQL Server settings** blade and complete the following:
    * SQL connectivity: **Public (Internet)**
    * Port: **1401** (to avoid using a well-known port name in the public scenario)
    * SQL Authentication: click **Enable**. 
8. Click **Review + create** to complete the configuration of the SQL Server VM. Once validation passes, click **Create** to build your VM.  Note that it may take 10-15 minutes to build out your virtual machine with SQL installed.

## Exercise 2 - Connect to the SQL VM

1. At the top of the Azure portal, enter **SQLVM**. When **SQLVM** appears in the search results, select it. Select the **Connect** button.
2. After selecting the Connect button, click on **Download RDP** file.
3. Enter the user name and password you specified when creating the VM. You may need to select **More choices** then **Use a different account** to specify the credentials you entered when you created the VM.
4. Select **OK**.
5. Once the desktop renders, click **No** on the Networks blade.

## Exercise 3 - Map a drive to an Azure Files Share

Before you can complete this section, you will need to map a drive to an Azure File Share to obtain sample data.

1. From **SQLVM**, open a command prompt (Start/Windows System).
2. Key in (cut and paste) the following and hit **enter**:

    `net use Z: \\wagsazurefiles.file.core.windows.net\sampledata /u:AZURE\wagsazurefiles tCfYh37xGNjIc0czqfTW9+kUHIIhlxRUPh9h4YtD/hh7FiFPn1v32RH7uV0a83E6nAa6kkVU6d+nAAeoBItpJg==`

3. Once Z: is mapped, change to the Z: drive.
4. Confirm that you can see a file named sampledata.txt.

## Exercise 4 - Connect to SQL

1. Back on the desktop of **SQLVM** click the **Start Button**,  under the letter "M" choose **Microsoft SQL Server Tools 18** and then **Microsoft SQL Server Management Studio**.  It might take a few seconds for Management Studio to fire up.
2. In the **Connect to Server** or **Connect to Database Engine** dialog box, ensure the Server name value is  **SQLVM**.
3. In the Authentication box, select **SQL Server Authentication**.
4. In the Login box, type the name of the SQL login you created during  the previous exercise.
5. In the Password box, type the password of the login from the previous exercise.
6. Click **Connect**.

## Exercise 5 - Create a new Database and Import Data

1. Under **SQLVM**, select and right-click **Databases** then **New Database ...**
2. Enter **SampleData** as the Database name and click **OK**.
3. Once the database is created, expand Databases and then right-click **SampleData**, then **Tasks**, then **Import Flat File**.
4. On the Introduction screen click **Next**.
5. On the **Specify Input File** screen select click **Browse**, then select  the Z: Drive, then **sampledata.txt**, then click **Open**, then click **Next**.
6. Review the information on the **Preview Data screen** and click **Next**.
7. On the **Modify Columns screen** set `zip` as the Primary Key, enable **Allow Nulls** on all columns, and then click **Next**.
8. Click **Finish** on the Summary screen.
9. Click **Close** on the Results screen.

## Exercise 6 - Install the Data Migration Assistant

1. Disable Internet Explorer ESC in Server Manager for Administrators.
2. From  **SQLVM** or your local Windows computer open a web browser.  Search for, download,  and install the [Data Migration Assistant (DMA)](https://www.microsoft.com/download/details.aspx?id=53595) v5.2 or later using the default settings.  If you are using a non-Windows computer, RDP into an existing VM from a previous lab and install DMA there.
2. On the Completed screen, check the box for **Launch Microsoft Data Migration Assistant** and click **Finish**.

## Exercise 7 - Assess the simulated on-premises database

Before you can migrate data from an on-premises SQL Server  to a single database or pooled database in Azure SQL Database, you need to assess the SQL Server database for any blocking issues that might prevent migration.

1. In DMA, select the  **+** icon, and then select  **Assessment** as the project type.
2. Specify **SampleAssessment** as the project name.
3. In the Source server type text box ensure that  **SQL Server** is selected, and in the Target server type text box, ensure **Azure SQL Database** is selected. Click  **Create** to create the project.
4. Select **Next** on the Options screen.
5. On the Select sources screen, enter the following and then select **Connect**:
    * Server name: Enter **SQLVM** if your built your own or `tiwsql.eastus2.cloudapp.azure.com, 1403` if you are using pre-built SQL.
    * Authentication type: **SQL Server Authentication**
    * In the **Username** box, type the name of a valid SQL login from the previous steps or **SQLAdmin** if you are using pre-built SQL.
    * In the **Password** box, type the password of the login from the previous steps or `Complex.Password` if you are using pre-built SQL.
    * Uncheck **Encrypt connection** under **Connection properties**.  Under normal circumstances we would use this option but we did not configure certificates on the source SQL server.
    * Click **Connect**.
6. Under *Add sources* select **SampleData** and the click **Add**.
7. Select **Start Assessment**.
8. Review the results and then select **Compatibility issues** radio button in the upper left-hand corner to see if there are any compatibility issues with your database.

## Exercise 8 -Provision an Azure SQL database

Before you migrate, a target Azure SQL database needs to be provisioned..

1. From the Azure Portal, Select **+Create a resource** in the upper left-hand corner of the Azure portal.
2. Select **Databases** and then select **SQL Database**.
3. In the **Create SQL Database** form, type or select the following values:
    * Resource group: *Create new*, type **SQL**.
    * Database name: Enter **Cloud**.
    * Server:  
        * Select *Create new*, type *yourinitials*+*todayshortdate*. Example: `abc032019`
        * Server admin login: *pick one* and write it down
        * Password: `Complex.Password`
        * Confirm Password: `Complex.Password`
        * Location: Select the same region as your other resources
        * Click **Ok**
    * Under **Compute + storage** click *Configure database*, choose **Basic**, and then click **Apply**.
4. Select **Review + Create** and then **Create**.
5. Once your database server is created, click **Go to resource**.
6. In the Azure Portal, copy to clipboard the FQDN of  the database server, such as `abc032019.database.windows.net`.
7. Open the server up to connections by:
    * Click on **Set Server firewall** in the Azure Portal.
    * Click **Yes** under **Allow Azure Services and resources to access this server**.
    * Create the following Rule:
        * Rule Name: **Everyone**
        * Start IP: **0.0.0.0**
        * End IP: **255.255.255.255**
    * Click **Save**

## Exercise 9 - Migrate the sample schema

After you're comfortable with the assessment and satisfied that the selected database is a viable candidate for migration to a single database or pooled database in Azure SQL Database, use DMA to migrate the schema to Azure SQL Database.

1. In the Data Migration Assistant, select the **New (+)** icon, and then under Project type, select **Migration**.
2. Specify the following and click **Create**.
    * **SQLMIG** as the project name
    * **SQL Server** in the Source server type text box
    * **Azure SQL Database** in the Target server type text box
3. Under Select source enter the following and click **Next**:
    * Server name: **SQLVM** or `tiwsql.eastus2.cloudapp.azure.com, 1403` if you are using pre-built SQL.
    * Authentication type: **SQL Server Authentication**
    * In the **Username** box, type the name of the SQL login you created or **SQLAdmin** if you are using pre-built SQL.
    * In the **Password** box enter `Complex.Password` if you are using pre-built SQL or the password you created.
    * De-select **Encrypt connection** and click **Connect**.
    * Uncheck **Assess database before migration?** and click **Next**.
4. On the **Select target** tab, enter the following:
    * Specify the source connection details for your SQL Server.  This is the FQDN name of the Azure SQL Database **YOU** created.
    * Authentication type: **SQL Server Authentication**
    * In the **Username** box, type the name of a valid SQL login from the previous steps.
    * In the **Password** box, type the password of the login from the previous steps.
    * Uncheck **Encryption connection**.
    * Click **Connect**.
5. Select **Cloud** when it appears and click **Next**.
6. On the **Select Objects tab** click **Generate SQL Script**.
7. On the **Script & deploy schema** tab, review the script and click **Deploy schema**.  Once complete, and that task should be fast, click **Migrate Data**.
8. On the **Select tables** tab, ensure that the row count is 38,803 and click **Start data migration**.
9. The migration should take less than 30 seconds.  Review the tab for deployment information on warnings, failures, etc.  

### **Congratulations, you have just migrated your first database to the cloud!**

## Exercise 10 - Validate your data in SQL

Let's make sure that your data got migrated and looks right.  We're going to connect to the Azure SQL database and example the data.

1. Switch to the **Microsoft SQL Server Management Studio** application.
2. Click on **File** then **Connect Object Explorer**.
3. Enter the following in the Connect to Server form and click **Connect**:
    * Server name: Enter your (Azure) SQL Server name. (e.g. `abc032019.database.windows.net`)
    * Authentication type: SQL Server Authentication
    * In the Username box, type the name of a valid SQL login from the previous steps.
    * In the Password box, type the password of the login from the previous steps.
4. Browse databases to select **Cloud**.
5. Right-click on **New Query** and enter the following:
    * `select TOP (1000) [zip] FROM dbo.sampledata`
6. Click **Execute**.  This query will search the Azure SQL database and display the first 1000 zip codes.

### **Congratulations for completing the SQL Migration lab!**
