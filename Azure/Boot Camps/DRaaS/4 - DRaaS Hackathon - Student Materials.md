# DRaaS Boot Camp Hackathon

In this exercise you are going to get practical experience designing a BCDR solution using various DRaaS components.

**Contents**

<!-- TOC -->

- [Business continuity and disaster recovery whiteboard design session student guide](#business-continuity-and-disaster-recovery-whiteboard-design-session-student-guide)
    - [Abstract](#abstract)
    - [Step 1: Review the customer case study](#step-1-review-the-customer-case-study)
        - [Customer situation](#customer-situation)
        - [Customer needs](#customer-needs)
        - [Customer objections](#customer-objections)
        - [Infographic for common scenarios](#infographic-for-common-scenarios)
    - [Step 2: Design a proof of concept solution](#step-2-design-a-proof-of-concept-solution)
    - [Step 3: Present the solution](#step-3-present-the-solution)
    - [Wrap-up](#wrap-up)
    - [Additional references](#additional-references)

<!-- /TOC -->

#  Business continuity and disaster recovery whiteboard design session student guide

## Abstract

In this virtual Whiteboard Design Session (vWDS), you will design a solution using Azure business continuity and disaster recovery (BCDR) technologies. Your solution will consider three different types of environments. The first will consist of on-premises VMs running applications that will be migrated to Azure IaaS. Next, Azure IaaS applications that need to be failed over from either on-premises to Azure, or between two Azure Regions. Finally, the use of automated failover technologies built into Azure PaaS services, App Service, and SQL Database will be used for PaaS applications.

At the end of this vWDS, you will be better able to design a solution that leverages various Azure technologies together to build a complex and robust IaaS BCDR plan.

## Step 1: Review the customer case study 

**Outcome**

Analyze your customer's needs.

Timeframe: 15 minutes

### Customer situation

Contoso Insurance (CI), headquartered in Miami, is a multinational corporation providing insurance solutions in North America, Europe, and Australia. Its products include accident and health insurance, life insurance, travel, home, and auto coverage. CI manages data collection services by sending mobile agents directly to the insured to gather information as part of the data collection process for claims from an insured individual. These mobile agents are based all over the world and are residents of the region in which they work. Mobile agents are managed remotely, and each regional corporate office has a support staff responsible for scheduling their time based on requests that arrive to the system.

CI currently hosts their primary corporate systems at co-location facilities within each geo-political region and manage all IT operations for the system. These sites include Miami, London, and Sydney all of which are connected to the Internet and each other via a combination of VPNs and private WANs.

CI is expecting significant growth within the United States and abroad. They foresee the need to scale their system, and their IT operations staff. "We are exploring a move to Microsoft Azure to simplify some of the operations management overhead and associated costs, beginning with our U.S. datacenter followed by those in Europe and Australia", says Liz Simmons, VP of Datacenters. Given CI's international footprint, they need to be operational 24x7. As such they have concerns about how they will address business continuity and disaster recovery (BCDR), in their move to the cloud.

CI has completed a cloud assessment of their applications and have classified their target applications into three types that they wish to focus on for their implementation of Azure BCDR technologies:

**Workgroup Applications:** These are typically smaller applications that run on a single VM and are used by 25 or fewer employees. These applications have been developed locally by the Regional IT teams and are based on Linux, Apache, PHP, and MySQL (LAMP). These applications are important to the various business units and need to be managed and require backup but won't need mission-critical failover capabilities. The primary concern is how to migrate these applications as quickly as possible to Azure with minimal downtime.

**Enterprise Applications:** The applications in this classification are mission critical to the business and have been developed using ASP.NET and SQL Server Enterprise over many years. They are deployed using Windows Clustering and SQL Always On Availability Groups (AOG), to ensure failover of databases during an outage. These applications are running in the various datacenters around the world but currently, have no DR capabilities. They are backed up using a third-party software with a mix of online disk backup and ultimately then archived to tape and sent offsite for storage. Given their status as core to the business, they require complex DR failover and failback capabilities whether they are running in an Azure region or one of the colocation facilities. "There is critical concern about the Miami datacenter given how hurricane-prone the area is and the intensity of the storms which could cause outages lasting weeks," says Clifton Quinlan Director of Continuity of Business (COB). "With only a few months until the next hurricane season, we need to get these applications protected in Azure. Some will be migrated immediately, and others will need to be extended to failover to the cloud but will need to allow for failback to the on-premises."

**Global, Mobile, and API Web Applications:** CI has leveraged their experience with ASP.NET and SQL Server to build applications that are Azure PaaS ready. These applications primarily service their external customers and the mobile agents. These include applications for consumers and their employees in the field dealing with claims. "We have prototyped these applications in Azure App Services and SQL Database with success but need a plan for how they will be implemented for high-availability and automatic failover," says Mrs. Simmons. "These applications are global, so we want to make sure that they are distributed around the world and that users will be directed to the closest point of presence (POP) but will never get an error if there is a local issue."

### Customer needs 

1.  CI needs to automate their backup and recovery of their solutions, not just individual components. They need a strategy not just point solutions. They are still backing up to tape and want to modernize this approach. The COB team is demanding that the recovery be testable before an event occurs.

2.  CI wants to insure they have the right breadth and depth of continuity protection. In the case of a lights-out event, they want to be able to control how the systems failover.

3.  CI wants to move very fast on some of their migrations to Azure. They need a plan for handling these migrations with very little downtime to the business.

4.  CI has struggled with DR solutions with respect to the Data tier of their application. They need to understand how this will work with IaaS and PaaS solutions for SQL Server and SQL DB.

5.  CI's corporate datacenter in the US is in a hurricane-prone region, and they need a backup datacenter that mirrors the core functions they have here. They don't want to build another datacenter.

### Customer objections 

1.  With the move to the cloud, they are uncomfortable with any situation that assumes the cloud provider will handle their fail-over.

2.  They have many systems that need to be accounted for and they aren't sure if the tools really exist to give them the business continuity they desire.

3.  They want to know their BCDR solution is secure.

4.  CI has heavily invested in a third-party backup solution but want to use Azure as their archive. Does Azure support this?

## Step 2: Design a proof of concept solution

**Outcome**

Design a solution and prepare to present the solution to the target customer audience in a 15-minute chalk-talk format.

Timeframe: 60 minutes

**Business needs**

Directions:  Answer the following questions and be preparred to discuss your answers:

1.  Who should you present this solution to? Who is your target customer audience? Who are the decision makers?

2.  What customer business needs do you need to address with your solution?

**Design**

Directions: Answer the following questions and be preparred to discuss your answers:

*Provide an overview of the technologies and the implementation at a high-level. How will you use Azure's BCDR technologies to meet the Customer's needs?*

1.  Azure Regions

    -   Which regions should be deployed in support of the goals of the client?

    -   Why did you select these particular regions?

2.  Backup

    -   How can each part of their solution be backed up?

    -   How can they sunset the use of tape archive?

    -   Can they continue to use their third-party backup vendor?

3.  Disaster Recovery

    -   What DR Solutions will be leveraged for the implementation at CI?

4.  Design a HA and BDCR solution for each of the three application classifications. At a high-level, provide details of your implementation. Make sure to document your design with a diagram along with addressing the questions.

-   Workgroup Applications

    -   What Azure BCDR technologies will you implement for this classification?

    -   How will you migrate these VMs to Azure?

    -   Can you test the migration before going live?

    -   Given this will be an IaaS implementation, provide details of how you will provide HA and Failover capabilities to these VMs once they are in Azure?

-   Enterprise Applications

    -   What Azure BCDR technologies will you implement for this classification?

    -   Document how you will implement both types of implementations:

        -  Migrate to Azure and support Azure region-to-region failover

        -  Remain on-premises for primary, but support Hyper-V to Azure Region Failover

    -   How will SQL Always On Availability Groups be set up in order to support both of these scenarios?

    -   What Azure technology, which is complimentary to DR, will you implement to deal with application-specific tasks such as Pre-Actions and Post-Actions during a failover?

    -   Given this will be an IaaS implementation, provide details of how you will provide high availability (HA) and failover capabilities to these VMs once they are in Azure?

    -   How will you direct web traffic to the active site?

-   Global, Mobile and API Web Applications

    -   What Azure BCDR technologies will you implement for this classification?

    -   Given this will be a PaaS implementation, provide details of how you will provide high availability (HA) and failover capabilities to these VMs once they are in Azure?

    -   How will SQL Database be set up to support both scenarios?

    -   What Azure technology and/or DevOps tools, will you implement to deal with application specific tasks such as Pre-Actions and Post-Actions during a failover?

    -   How will you direct web traffic to the different POPs around the world?

**Customer Objections**

1.  Provide details on how you will address each of the objections that were put forward by the client.

**Prepare**

Directions: Answer the following questions and be preparred to discuss your answers:

1.  Identify any customer needs that are not addressed with the proposed solution.

2.  Identify the benefits of your solution.

3.  Determine how you will respond to the customer's objections.

Prepare a 15-minute chalk-talk style presentation to the customer.

## Step 3: Price the solution

**Outcome**

Develop a ROM price by using the Azure Calcuator and PAYG pricing. 
Timeframe: 30 minutes

**Presentation**

Directions:

1.  Use the [Azure Calculator](https://azure.microsoft.com/en-us/pricing/calculator/).

2. 

##  Wrap-up 

Timeframe: 15 minutes

Directions: Discuss as a group the preferred solution for the case study.

##  Additional references
|    |            |       
|----------|:-------------:|
| **Description** | **Links** |
| Overview of Azure Site Recovery | <https://docs.microsoft.com/en-us/azure/site-recovery/site-recovery-overview/> |
| Prepare Azure resources for replication of on-premises machines | <https://docs.microsoft.com/en-us/azure/site-recovery/tutorial-prepare-azure/> |
| Migrate on-premises machines to Azure | <https://docs.microsoft.com/en-us/azure/site-recovery/tutorial-migrate-on-premises-to-azure/> |
| Azure to Azure replication architecture | <https://docs.microsoft.com/en-us/azure/site-recovery/concepts-azure-to-azure-architecture/> |
| Hyper-V to Azure replication architecture | <https://docs.microsoft.com/en-us/azure/site-recovery/concepts-hyper-v-to-azure-architecture/> |
| Azure to Azure Support Matrix | <https://docs.microsoft.com/en-us/azure/site-recovery/site-recovery-support-matrix-azure-to-azure/> |
| Support matrix for Hyper-V replication to Azure | <https://docs.microsoft.com/en-us/azure/site-recovery/support-matrix-hyper-v-to-azure/> |
| Protect SQL Server using SQL Server disaster recovery and Azure Site Recovery | <https://docs.microsoft.com/en-us/azure/site-recovery/site-recovery-sql/> |
| Overview: Failover groups and active geo-replication | <https://docs.microsoft.com/en-us/azure/sql-database/sql-database-geo-replication-overview/> |
| What is Traffic Manager? | <https://docs.microsoft.com/en-us/azure/traffic-manager/traffic-manager-overview/> |
| Traffic Manager Routing Methods | <https://docs.microsoft.com/en-us/azure/traffic-manager/traffic-manager-routing-methods/> |
| Traffic Manager Endpoints | <https://docs.microsoft.com/en-us/azure/traffic-manager/traffic-manager-endpoint-types/> |
| Backup Solution Architectures | <https://azure.microsoft.com/en-us/solutions/architecture/backup-archive-on-premises-applications/> |

