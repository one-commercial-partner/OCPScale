# Design a Business Continuity Strategy (15-20%)

## Design a Site Recovery Strategy

Read the [Azure Site Recovery Overview](https://docs.microsoft.com/en-us/azure/site-recovery/site-recovery-overview) and [Design a recovery solution](https://docs.microsoft.com/en-us/azure/architecture/resiliency/disaster-recovery-azure-applications) articles.

| Topic | Link |
| - | - |
|design a recovery solution|[About Site Recovery](https://docs.microsoft.com/en-us/azure/site-recovery/site-recovery-overview)|
| |[General questions about Azure Site Recovery](https://docs.microsoft.com/en-us/azure/site-recovery/site-recovery-faq)
| design a site recovery replication policy|[Configure and manage replication policies for VMware disaster recovery](https://docs.microsoft.com/en-us/azure/site-recovery/vmware-azure-set-up-replication)|
| design for site recovery capacity|[Plan capacity and scaling for VMware disaster recovery to Azure](https://docs.microsoft.com/en-us/azure/site-recovery/site-recovery-plan-capacity-vmware)|
| design for storage replication|[Design site failover and failback (planned/unplanned)](https://docs.microsoft.com/en-us/azure/site-recovery/site-recovery-create-recovery-plans)|
|design site failover and failback|[Fail over and reprotect Azure VMs between regions](https://docs.microsoft.com/en-us/azure/site-recovery/azure-to-azure-tutorial-failover-failback)|
| |[Failback of VMware VMs after disaster recovery to Azure](https://docs.microsoft.com/en-us/azure/site-recovery/concepts-types-of-failback)|
|design the site recovery Network|Read all sections of: [About networking in Azure VM disaster recovery](https://docs.microsoft.com/en-us/azure/site-recovery/azure-to-azure-about-networking)|
|recommend recovery objectives (Azure, on-prem, hybrid, Recovery Time Objective (RTO), Recovery Level Objective (RLO), Recovery Point Objective (RPO))| [About Site Recovery](https://docs.microsoft.com/en-us/azure/site-recovery/site-recovery-overview)|
| |[Overview of the resiliency pillar](https://docs.microsoft.com/en-us/azure/architecture/resiliency/)|
|identify resources that require site recovery|[Identify resources that require site recovery](https://docs.microsoft.com/en-us/azure/sql-data-warehouse/sql-data-warehouse-overview-what-is)|
|identify supported and unsupported workloads|Visit [Azure Site Recovery documentation](https://docs.microsoft.com/en-us/azure/site-recovery/) and search for "support" to review all support matrices
|recommend a geographical distribution strategy|[Move Azure VMs to another region](https://docs.microsoft.com/en-us/azure/site-recovery/azure-to-azure-tutorial-migrate)|
|Experiential Learning|[Protect your on-premises infrastructure from disasters with Azure Site Recovery](https://docs.microsoft.com/en-us/learn/modules/protect-on-premises-infrastructure-with-azure-site-recovery/)|
| |[Protect your Azure infrastructure with Azure Site Recovery](https://docs.microsoft.com/en-us/learn/modules/protect-infrastructure-with-site-recovery/)|
| |[Design your site recovery solution in Azure](https://docs.microsoft.com/en-us/learn/modules/design-your-site-recovery-solution-in-azure/)

## Design for High Availability

Read the following to prepare for this section of the exam:

[Overview of the resiliency pillar](https://docs.microsoft.com/en-us/azure/architecture/checklist/resiliency)

[Resiliency checklist for specific Azure services](https://docs.microsoft.com/en-us/azure/architecture/checklist/resiliency-per-service)

[Using business metrics to design resilient Azure applications](https://docs.microsoft.com/en-us/azure/architecture/framework/resiliency/business-metrics)

[Testing Azure applications for resiliency and availability](https://docs.microsoft.com/en-us/azure/architecture/framework/resiliency/testing)


* design for application redundancy||
* design for autoscaling
* design for data center and fault domain redundancy
* design for network redundancy
* identify resources that require high availability
* identify storage types for high availability
* design a disaster recovery strategy for individual workloads
* design failover/failback scenarios
* document recovery requirements
* identify resources that require backup
* recommend a geographic availability strategy

## Design a Data Archiving Strategy

| Topic | Link |
| - | - |
|recommend storage types and methodology for data archiving|[Azure Blob storage: hot, cool, and archive access tiers](https://docs.microsoft.com/en-us/azure/storage/blobs/storage-blob-storage-tiers)|
|identify business compliance requirements for data archiving|[Azure Storage compliance offerings](https://docs.microsoft.com/en-us/azure/storage/common/storage-compliance-offerings)|
|identify requirements for data archiving|[Backup and archive SOlution Architectures](https://azure.microsoft.com/en-us/solutions/backup-archive/#references)|
|identify SLA(s) for data archiving|[SLA for Storage Accounts](https://azure.microsoft.com/en-us/support/legal/sla/storage/v1_2/)|
| |[Azure Blob storage: hot, cool, and archive access tiers](https://docs.microsoft.com/en-us/azure/storage/blobs/storage-blob-storage-tiers?tabs=azure-portal)|


[Back](index.md)
