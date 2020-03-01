# AZ-300: Microsoft Azure Architect Technologies Study Group
## Develop for the cloud and for Azure storage (15-20%)

### Configure a message-based integration architecture
- configure an app or service to send emails
  - [How to Send Email Using SendGrid with Azure](https://docs.microsoft.com/en-us/azure/sendgrid-dotnet-how-to-send-email)
  - [Logic Apps: How to send a well-formatted HTML Email notification with Office 365 Outlook connector](https://blog.sandro-pereira.com/2020/01/26/logic-apps-how-to-send-a-well-formatted-html-email-notification-with-office-365-outlook-connector/)
- configure Event Grid
  - [What is Azure Event Grid?](https://docs.microsoft.com/en-us/azure/event-grid/overview)
  - [Quickstart: Route custom events to web endpoint with the Azure portal and Event Grid](https://docs.microsoft.com/en-us/azure/event-grid/custom-event-quickstart-portal)
- configure the Azure Relay service
  - [What is Azure Relay?](https://docs.microsoft.com/en-us/azure/service-bus-relay/relay-what-is-it)
  - [Get started with Relay Hybrid Connections HTTP requests in .NET](https://docs.microsoft.com/en-us/azure/service-bus-relay/relay-hybrid-connections-http-requests-dotnet-get-started)
- create and configure a Notification Hub
  - [What is Azure Notification Hubs?](https://docs.microsoft.com/en-us/azure/notification-hubs/notification-hubs-push-notification-overview)
  - [Create an Azure notification hub in the Azure portal](https://docs.microsoft.com/en-us/azure/notification-hubs/create-notification-hub-portal)
- create and configure an Event Hub
  - [Azure Event Hubs — A big data streaming platform and event ingestion service](https://docs.microsoft.com/en-us/azure/event-hubs/event-hubs-about)
  - [Quickstart: Create an event hub using Azure portal](https://docs.microsoft.com/en-us/azure/event-hubs/event-hubs-create)
- create and configure a Service Bus
  - [What is Azure Service Bus?](https://docs.microsoft.com/en-us/azure/service-bus-messaging/service-bus-messaging-overview)
  - [Quickstart: Use Azure portal to create a Service Bus queue](https://docs.microsoft.com/en-us/azure/service-bus-messaging/service-bus-quickstart-portal)
  - [Quickstart: Use the Azure portal to create a Service Bus topic and subscriptions to the topic](https://docs.microsoft.com/en-us/azure/service-bus-messaging/service-bus-quickstart-topics-subscriptions-portal)
- configure queries across multiple products
  - TBD

### Develop for autoscaling
- implement autoscaling rules and patterns (schedule, operational/system metrics)
  - [Overview of autoscale in Microsoft Azure Virtual Machines, Cloud Services, and Web Apps](https://docs.microsoft.com/en-us/azure/azure-monitor/platform/autoscale-overview)
  - [Autoscaling](https://docs.microsoft.com/en-us/azure/architecture/best-practices/auto-scaling)
  - [Best practices for Autoscale](https://docs.microsoft.com/en-us/azure/azure-monitor/platform/autoscale-best-practices)
  - [Overview of autoscale with Azure virtual machine scale sets](https://docs.microsoft.com/en-us/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-autoscale-overview)
- implement code that addresses singleton application instances
  - [Design to scale out](https://docs.microsoft.com/en-us/azure/architecture/guide/design-principles/scale-out)
- implement code that addresses transient state
  - [Transient fault handling](https://docs.microsoft.com/en-us/azure/architecture/best-practices/transient-faults)

### Develop solutions that use Cosmos DB storage
- create, read, update, and delete data by using appropriate APIs
  - [Welcome to Azure Cosmos DB](https://docs.microsoft.com/en-us/azure/cosmos-db/introduction)
  - [Understanding the differences between NoSQL and relational databases](https://docs.microsoft.com/en-us/azure/cosmos-db/relational-nosql)
  - [SQL API](https://docs.microsoft.com/en-us/azure/cosmos-db/how-to-sql-query)
  - [Cassandra API](https://docs.microsoft.com/en-us/azure/cosmos-db/cassandra-introduction)
  - [MongoDB API](https://docs.microsoft.com/en-us/azure/cosmos-db/mongodb-introduction)
  - [Gremlin API](https://docs.microsoft.com/en-us/azure/cosmos-db/graph-introduction)
  - [Table Storage API](https://docs.microsoft.com/en-us/azure/cosmos-db/table-introduction)
- implement partitioning schemes
  - [Partitioning in Azure Cosmos DB](https://docs.microsoft.com/en-us/azure/cosmos-db/partitioning-overview)
  - [Partitioning and horizontal scaling in Azure Cosmos DB](https://docs.microsoft.com/en-us/azure/cosmos-db/partition-data)
- set the appropriate consistency level for operations
  - [Consistency levels in Azure Cosmos DB](https://docs.microsoft.com/en-us/azure/cosmos-db/consistency-levels)
  - [Consistency, availability, and performance tradeoffs](https://docs.microsoft.com/en-us/azure/cosmos-db/consistency-levels-tradeoffs)
  
### Develop solutions that use a relational database
- provision and configure relational databases
  - [Choose the right deployment option in Azure SQL](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-paas-vs-sql-server-iaas)
  - [What is the Azure SQL Database service?](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-technical-overview)
  - [Getting started with single databases in Azure SQL Database](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-single-database-quickstart-guide)
- configure elastic pools for Azure SQL Database
  - [Elastic pools help you manage and scale multiple Azure SQL databases](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-elastic-pool)
- implement Azure SQL Database managed instances
  - [Getting started with Azure SQL Database managed instance](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-managed-instance-quickstart-guide)
- create, read, update, and delete data tables by using code
  - [Build an app using SQL Server](https://www.microsoft.com/en-us/sql-server/developer-get-started/)

[Back](index.md)