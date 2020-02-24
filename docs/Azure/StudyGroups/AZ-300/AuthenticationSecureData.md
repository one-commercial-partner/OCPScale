# AZ-300: Microsoft Azure Architect Technologies Study Group
## Implement authentication and secure data (5-10%)

### Implement authentication
- implement authentication by using certificates, forms-based authentication, tokens, or Windows-integrated authentication
  - [What is Azure Active Directory authentication?](https://docs.microsoft.com/en-us/azure/active-directory/authentication/overview-authentication)
  - [Authentication and authorization in Azure App Service](https://docs.microsoft.com/en-us/azure/app-service/overview-authentication-authorization)
  - [Get started with certificate-based authentication in Azure Active Directory](https://docs.microsoft.com/en-us/azure/active-directory/authentication/active-directory-certificate-based-authentication-get-started)
  - [Windows Authentication and Azure Multi-Factor Authentication Server](https://docs.microsoft.com/en-us/azure/active-directory/authentication/howto-mfaserver-windows)
  - [Advanced certificate signing options in the SAML token for gallery apps in Azure Active Directory](https://docs.microsoft.com/en-us/azure/active-directory/manage-apps/certificate-signing-options)
- implement multi-factor authentication by using Azure AD
  - [How it works: Azure Multi-Factor Authentication](https://docs.microsoft.com/en-us/azure/active-directory/authentication/concept-mfa-howitworks)
- implement OAuth2 authentication
  - [Understanding the OAuth2 implicit grant flow in Azure Active Directory (AD)](https://docs.microsoft.com/en-us/azure/active-directory/azuread-dev/v1-oauth2-implicit-grant-flow)
- implement Managed Identities for Azure resources Service Principal authentication
  - [What are managed identities for Azure resources?](https://docs.microsoft.com/en-us/azure/active-directory/managed-identities-azure-resources/overview)
  - [Use a Windows VM system-assigned managed identity to access Resource Manager](https://docs.microsoft.com/en-us/azure/active-directory/managed-identities-azure-resources/tutorial-windows-vm-access-arm)
  - [Use a Linux VM system-assigned managed identity to access Azure Resource Manager](https://docs.microsoft.com/en-us/azure/active-directory/managed-identities-azure-resources/tutorial-linux-vm-access-arm)

### Implement secure data solutions
- encrypt and decrypt data at rest and in transit
  - [Azure Data Encryption-at-Rest](https://docs.microsoft.com/en-us/azure/security/fundamentals/encryption-atrest)
  - [How Does Azure Encrypt Data?](https://cloudacademy.com/blog/how-does-azure-encrypt-data/)
- encrypt data with Always Encrypted
  - [Transparent data encryption or always encrypted?](https://azure.microsoft.com/en-us/blog/transparent-data-encryption-or-always-encrypted/)
  - [Always Encrypted: Protect sensitive data and store encryption keys in the Windows certificate store](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-always-encrypted)
  - [Always Encrypted: Protect sensitive data and store encryption keys in Azure Key Vault](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-always-encrypted-azure-key-vault?tabs=azure-powershell)
- implement Azure Confidential Compute
  - [Azure confidential computing](https://azure.microsoft.com/en-us/blog/azure-confidential-computing/)
  - [Azure Confidential Computing updates with Mark Russinovich | Best of Microsoft Ignite 2018](https://www.youtube.com/watch?v=Qu6sP0XDMU8)
- implement SSL/TLS communications
  - [Azure data security and encryption best practices](https://docs.microsoft.com/en-us/azure/security/fundamentals/data-encryption-best-practices)
- create, read, update, and delete keys, secrets, and certificates by using the KeyVault API
  - [What is Azure Key Vault?](https://docs.microsoft.com/en-us/azure/key-vault/key-vault-overview)
  - [Azure Key Vault basic concepts](https://docs.microsoft.com/en-us/azure/key-vault/basic-concepts)
  - [Azure Key Vault Developer's Guide](https://docs.microsoft.com/en-us/azure/key-vault/key-vault-developers-guide)
  - [Azure Key Vault REST API reference](https://docs.microsoft.com/en-us/rest/api/keyvault/) Review each command (e.g. create, read, etc.) in the list 

[Back](index.md)