# Secure data and applications (30-35%)

## Configure security policies to manage data

Read the whitepaper: [Achieving Compliant Data Residency and Security with Azure](https://azure.microsoft.com/en-us/resources/achieving-compliant-data-residency-and-security-with-azure/)

| Topic | Link |
| --- | --- |
|configure data classification|[What is data classification?](https://docs.microsoft.com/en-us/azure/cloud-adoption-framework/govern/policy-compliance/data-classification)
|configure data retention|[Explore data recovery, retention, and disposal](https://docs.microsoft.com/en-us/learn/modules/configure-security-policies-to-manage-data/4-configure-data-retention)|
|configure data sovereignty|[Custom Data Sovereignty & Data Gravity Requirements](https://docs.microsoft.com/en-us/azure/architecture/solution-ideas/articles/data-sovereignty-and-gravity)

## Configure security for data infrastructure

| Topic | Link |
| --- | --- |
|enable database authentication|[Playbook for Addressing Common Security Requirements with Azure SQL Database](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-security-best-practice)
|enable database auditing|[Azure SQL Auditing](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-auditing)|
|configure Microsoft Azure SQL Database threat detection|[Azure SQL Database Advanced Threat Protection for single or pooled databases](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-threat-detection)
|configure access control for storage accounts|[Control access to account data](https://docs.microsoft.com/en-us/azure/storage/common/storage-account-overview#control-access-to-account-data)|
| |[Security recommendations for Blob storage](https://docs.microsoft.com/en-us/azure/storage/blobs/security-recommendations)|
|configure key management for storage accounts|[What is Azure Key Vault?](https://docs.microsoft.com/en-us/azure/key-vault/key-vault-overview)|
| |[Quickstart: Set and retrieve a secret from Azure Key Vault using Azure CLI](https://docs.microsoft.com/en-us/azure/key-vault/quick-create-cli)|
| |[Set up Azure Key Vault with key rotation and auditing](https://docs.microsoft.com/en-us/azure/key-vault/key-vault-key-rotation-log-monitoring)|
| |[Best practices to use Key Vault](https://docs.microsoft.com/en-us/azure/key-vault/key-vault-best-practices)|
| |[Configure customer-managed keys with Azure Key Vault by using the Azure portal](https://docs.microsoft.com/en-us/azure/storage/common/storage-encryption-keys-portal)|
|create and manage Shared Access Signatures (SAS)|[Grant limited access to Azure Storage resources using shared access signatures (SAS)](https://docs.microsoft.com/en-us/azure/storage/common/storage-sas-overview?toc=/azure/storage/blobs/toc.json)|
| |[Create a user delegation SAS for a container or blob with PowerShell](https://docs.microsoft.com/en-us/azure/storage/blobs/storage-blob-user-delegation-sas-create-powershell)|
|configure security for HDInsights|[Overview of enterprise security in Azure HDInsight](https://docs.microsoft.com/en-us/azure/hdinsight/domain-joined/hdinsight-security-overview)|
| |[Azure Security Baseline for HDInsight](https://docs.microsoft.com/en-us/azure/hdinsight/security-baseline)|
|configure security for Cosmos DB|[Security in Azure Cosmos DB - overview](https://docs.microsoft.com/en-us/azure/cosmos-db/database-security)|
| |[Azure Security Baseline for Cosmos DB](https://docs.microsoft.com/en-us/azure/cosmos-db/security-baseline)|
| |[Secure access to data in Azure Cosmos DB](https://docs.microsoft.com/en-us/azure/cosmos-db/secure-access-to-data)|
|configure security for Microsoft Azure Data Lake|[Security in Azure Data Lake Storage Gen1](https://docs.microsoft.com/en-us/azure/data-lake-store/data-lake-store-security-overview)|
| |[Virtual network integration for Azure Data Lake Storage Gen1](https://docs.microsoft.com/en-us/azure/data-lake-store/data-lake-store-network-security)|
| |[Securing data stored in Azure Data Lake Storage Gen1](https://docs.microsoft.com/en-us/azure/data-lake-store/data-lake-store-secure-data)|
| |[Access control in Azure Data Lake Storage Gen2](https://docs.microsoft.com/en-us/azure/storage/blobs/data-lake-storage-access-control)|

## Configure encryption for data at rest

| Topic | Link |
| --- | --- |
|implement Microsoft Azure SQL Database Always Encrypted|[Always Encrypted](https://docs.microsoft.com/en-us/sql/relational-databases/security/encryption/always-encrypted-database-engine?view=sql-server-ver15)
| |[Always Encrypted: Protect sensitive data and store encryption keys in the Windows certificate store](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-always-encrypted)|
| |[Always Encrypted: Protect sensitive data and store encryption keys in Azure Key Vault](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-always-encrypted-azure-key-vault?tabs=azure-powershell)|
|implement database encryption|[Azure data security and encryption best practices](https://docs.microsoft.com/en-us/azure/security/fundamentals/data-encryption-best-practices)|
|implement Storage Service Encryption|[Azure Storage encryption for data at rest](https://docs.microsoft.com/en-us/azure/storage/common/storage-service-encryption)|
|implement disk encryption|[Azure Disk Encryption for virtual machines and virtual machine scale sets](https://docs.microsoft.com/en-us/azure/security/fundamentals/azure-disk-encryption-vms-vmss)
|implement backup encryption|[Back up and restore encrypted Azure VM](https://docs.microsoft.com/en-us/azure/backup/backup-azure-vms-encryption)|

## Implement security for application delivery

| Topic | Link |
| --- | --- |
|implement security validations for application development|[Develop secure applications on Azure](https://docs.microsoft.com/en-us/azure/security/develop/secure-develop)|
| |[Design secure applications on Azure](https://docs.microsoft.com/en-us/azure/security/develop/secure-design)|
| |[Deploy secure applications on Azure](https://docs.microsoft.com/en-us/azure/security/develop/secure-deploy)|
| |[Secure development best practices on Azure](https://docs.microsoft.com/en-us/azure/security/develop/secure-dev-overview)
|configure synthetic security transactions|[Synthetic Web Test for Microsoft OMS](https://gallery.technet.microsoft.com/Synthetic-Web-Test-for-OMS-1dd5e44d)

## Configure application security

| Topic | Link |
| --- | --- |
|configure SSL/TLS certs|[Certificates overview for Azure Cloud Services](https://docs.microsoft.com/en-us/azure/cloud-services/cloud-services-certs-create)|
| |[Configuring TLS for an application in Azure](https://docs.microsoft.com/en-us/azure/cloud-services/cloud-services-configure-ssl-certificate-portal)|
|configure Microsoft Azure services to protect web apps|[Threat Modeling Fundamentals](https://docs.microsoft.com/en-us/learn/paths/tm-threat-modeling-fundamentals/)|
|create an application security baseline|[Create security baselines](https://docs.microsoft.com/en-us/learn/modules/create-security-baselines/)

## Configure and manage Key Vault

| Topic | Link |
| --- | --- |
|manage access to Key Vault|[Secure access to a key vault](https://docs.microsoft.com/en-us/azure/key-vault/key-vault-secure-your-key-vault)|
| |[Configure Azure Key Vault firewalls and virtual networks](https://docs.microsoft.com/en-us/azure/key-vault/key-vault-network-security)
|manage permissions to secrets, certificates, and keys|[Provide Key Vault authentication with an access control policy](https://docs.microsoft.com/en-us/azure/key-vault/key-vault-group-permissions-for-apps)|
| |[Identity and access management](https://docs.microsoft.com/en-us/azure/key-vault/overview-security#identity-and-access-management)|
|manage certificates|[Monitor and manage certificate creation](https://docs.microsoft.com/en-us/azure/key-vault/create-certificate-scenarios)|
|manage secrets|[About keys, secrets, and certificates](https://docs.microsoft.com/en-us/azure/key-vault/about-keys-secrets-and-certificates)
|configure key rotation|[Set up Azure Key Vault with key rotation and auditing](https://docs.microsoft.com/en-us/azure/key-vault/key-vault-key-rotation-log-monitoring)|
Experiential Learning|[Configure and manage secrets in Azure Key Vault](https://docs.microsoft.com/en-us/learn/modules/configure-and-manage-azure-key-vault/)|
| |[Manage secrets in your server apps with Azure Key Vault](https://docs.microsoft.com/en-us/learn/modules/manage-secrets-with-azure-key-vault/)|

[Back](index.md)
