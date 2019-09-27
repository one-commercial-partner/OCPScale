# Azure Migration

## Database Migration Service
In this lab you are going to create an IaaS VM with SQL as a source environment, load the database up with data, then use DMS to migrate that over to Azure.

## Task 1 - Create a SQL Server VM image
1. Log in to the Azure portal using your account.
2. On the Azure portal, click **+Create a resource**.
3. In the search field, type **SQL Server 2017 Developer on Windows Server 2016** and press **ENTER**.
4. Select the **Free SQL Server License: SQL Server 2017 Developer on Windows Server 2016 image** and click **Create**.
5. Enter the following and then click **OK**.
    * Name: **SQLVM**
    * Username: `pick one` *and write it down*
    * Password: `Complex.Password`
    * Confirm Password: `Complex.Password`
    * Resource Group: *create new* **SQLMIG**
6. On the **Choose a size**  blade select **D2S_V3** and then click **Select**.
7. On the **Settings** blade select **RDP** under **Select public inbound ports** and click **OK**.
8. On the **Configure SQL Server settings** blade complete the following:
    * In the SQL connectivity drop-down, select **Public (Internet)**. This allows SQL Server connections over the internet.
    * Change the Port to **1401** to avoid using a well-known port name in the public scenario.
    * Under **SQL Authentication**, click **Enable**. The SQL Login is set to the same user name and password that you configured for the VM.
    * Click **OK** to complete the configuration of the SQL Server VM.
9. Once validation completes, click **Create** to build your VM.  Note that it may take 10-15 minutes to build out your VM 

## Task 2 - Connect to VM 
1. At the top of the Azure portal, enter **SQLVM**. When **SQLVM** appears in the search results, select it. Select the **Connect** button.
2. After selecting the Connect button, click on **Download RDP** file.
3. Enter the user name and password you specified when creating the VM. You may need to select **More choices**, then **Use a different account,** to specify the credentials you entered when you created the VM.
4. Select **OK**.
5. Once the desktop renders, click **No** on the Networks blade.

## Task 3 - Map a drive to an Azure Files Share 
Before you can complete this section, you will need to map a drive to an Azure File Share to obtain sample data.
1. Open a command prompt.
2. Key in (cut and paste) the following and hit enter:
    * *net use Z: \\\wagsazurefiles.file.core.windows.net\sampledata /u:AZURE\wagsazurefiles tCfYh37xGNjIc0czqfTW9+kUHIIhlxRUPh9h4YtD/hh7FiFPn1v32RH7uV0a83E6nAa6kkVU6d+nAAeoBItpJg==*
3. Once Z: is mapped change to the Z: drive.
4. Confirm that you can see a file named sampledata.txt


## Task 4 - Connect to SQL
1. Back on the desktop click the **Start Button**,  under the letter "M" choose **Microsoft SQL Server Management Studio** under **Microsoft SQL Server Tools 17**.
3. In the **Connect to Server** or **Connect to Database Engine** dialog box, edit the Server name value. Enter your VM's name.
4. In the Authentication box, select **SQL Server Authentication**.
5. In the Login box, type the name of a valid SQL login from  the previous section.
6. In the Password box, type the password of the login from  the previous section.
7. Click **Connect**.

## Task 5 - Create a new Database and Import Data
1. Under **SQLVM**, right-click **Databases** then **New Database ...**
2. Enter **SampleData** as the Database name and click **OK**.
3. Once the database is created, right-click **SampleData**, then **Tasks**, then **Import Flat File**
4. On the Introduction screen click **Next**.
5. On the **Specify Input File** screen select click **Browse**, then select  the Z: Drive, then **sampledata.txt**, then click **Open**.  Click **Next**.
6. Review the information on the **Preview Data screen** and click **Next**.
7. On the **Modify Columns screen** set *zip* as the Primary Key, enable **Allow Nulls** on all columns, and then click **Next**.
8. Click **Finish** on the Summary screen.
9. Click **Close** on the Results screen

## Task 6 - Install the Data Migration Assistant
1. From the SQLVM, download and install the [Data Migration Assistant (DMA)](https://www.microsoft.com/download/details.aspx?id=53595) v3.3 or later using the defaults.
2. On the Completed screen, check the box for **Launch Microsoft Data Migration Assistant** and click **Finish**.

## Task 7 - Assess your on-premises database
Before you can migrate data from an on-premises SQL Server instance to a single database or pooled database in Azure SQL Database, you need to assess the SQL Server database for any blocking issues that might prevent migration. 

1. In DMA, select the  (+) icon, and then select  **Assessment** as the project type.
2. Specify **SampleDataAssessment** as the project name.
3.  in the Source server type text box, select **SQL Server**, and in the Target server type text box, select **Azure SQL Database**. Select **Create** to create the project.
4.  Select **Next** on the Options screen.
5. On the Select sources screen, enter the following and then select **Connect**:
    * Server name: Enter your VM's server name.
    * Uncheck **Encrypt connection** under **Connection properties**.  Under normal circumstances we would use this option but we did not configure certificates on the source SQL server.
    * Authentication type: **SQL Server Authentication**
    * In the **Username** box, type the name of a valid SQL login from the previous steps.
    * In the **Password** box, type the password of the login from the previous steps.
    *  Click **Connect**.
6. Select **SampleData** and the click **Add**.
7. Select **Begin Assessment**.
8. Review the results and the select **Compatibility issues** radio button in the upper left ahnd corner to see if there are any compatibility issues with your database.


## Task 8 -Provisioned an Azure SQL database
Before you create a migration project in DMA, be sure that you have already provisioned an Azure SQL database.

1. Select **Create a resource** in the upper left-hand corner of the Azure portal.
2. Select **Databases** and then select **SQL Database**.
3. In the **Create SQL Database form**, type or select the following values: 
    * Resource group: Select *Create new*, type **MySQLDBs**. 
    * Database name: Enter **mySampleDatabase**.
    * Server:  
        * Select *Create new*, type *yourinitials*+*todayshortdate*. Example: `abc032019`
        *  Server admin login: *pick one* and write it down
        * Password: `Complex.Password`
        * Confirm Password: `Complex.Password`
        * Click **Select**
    * Under **Compute + storage** click *Configure database*, choose **Basic**, and then click **Apply**.
4. Select **Review + Create** and the **Create**.
5. Once you database server is created, click on the server in the deployment tab.
6. In the Azure Portal, click on **MySampleDatabase** and copy the FQDN to the database server, such as abc1234.database.windows.net.



## Task 9 - Migrate the sample schema
After you're comfortable with the assessment and satisfied that the selected database is a viable candidate for migration to a single database or pooled database in Azure SQL Database, use DMA to migrate the schema to Azure SQL Database.

1. In the Data Migration Assistant, select the **New (+) **icon, and then under Project type, select **Migration**.
2. Specify **SQLMIG** as the project name, in the Source server type text box, select **SQL Server**, and then in the Target server type text box, select **Azure SQL Database**.  Click **Create**.
3. In DMA, specify the source connection details for your SQL Server and select Connect.  This is the FQDN name you copied in Task 9.
4.  The SampleData database should be listed.  Uncheck **Assess database before migration?** and click Next.
5. On the Select Target tab, enter the floowing and click Next:
    * Server name: Enter your (Azure) SQL Server name. 
    * Authentication type: SQL Server Authentication
    * In the Username box, type the name of a valid SQL login from the previous steps.
    * In the Password box, type the password of the login from the previous steps.
    * Uncheck Encrypt connection under Connection properties. Under normal circumstances we would use this option but we did not configure certificates on the source SQL server.
6. Select **MySampleDB** when it appears and click **Next**.
7. On the **Select Objects tab** click **Generate SQL Script**.
8. On the **Script & deploy schema tab** review the script and click **Deploy schema**.  Once complete, and that tasks should be fast, click Migrate Data.
9. On the Slect tables tab, ensure that the row count is 38,803 and click **Start data migration**.
10. THe migration should take less than 30 seconds.  Review the tab for deployment information on warnings, failures, etc.  

Congratulations, you have just migrated your first database to the cloud!

## Task 10 - Validate your data in SQL
Let's make sure that your data got migrated and looks right.  We're going to connect to the Azure SQL database and example the data.

1. Switch to the **Microsoft SQL Server Management Studio** application.
2. Click on **File** then **Connection Object Explorer**.
3. Enter the following in the Connect to Server form and click COnnect:
    *  Server name: Enter your (Azure) SQL Server name. (e.g. `abc123.database.windows.net`)
    * Authentication type: SQL Server Authentication
    * In the Username box, type the name of a valid SQL login from the previous steps.
    * In the Password box, type the password of the login from the previous steps.
4. Change your available database to *MySampleDatabase*.
5. Click on **New Query** and enter the following:
    * select TOP (1000) [zip] FROM MySampleDatabase.dbo.sampledata
5. Click **Execute**.  This query will search the Azure SQL database and display the first 1000 zip codes.
