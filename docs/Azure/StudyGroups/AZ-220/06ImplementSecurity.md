# AZ-220 Process and Manage Data (15-20%)

* [AZ-220: Microsoft Azure IoT Developer Exam](https://docs.microsoft.com/en-us/learn/certifications/exams/az-220)
* [Microsoft Certified: Azure IoT Developer Specialty](https://docs.microsoft.com/en-us/learn/certifications/azure-iot-developer-specialty)
* [Yammer Azure Study Group](http://aka.ms/azurecsg)

## Skills Measured:
### Configure routing in Azure IoT Hub
* Implement message enrichment in IoT Hub
* Configure routing of IoT Device messages to endpoints
* Define and test routing queries
* Integrate with Event Grid

### Configure stream processing
* Create Azure Stream Analytics (ASA) for data and stream processing of IoT data
* Process and filter IoT data by using Azure Functions
* Configure Stream Analytics outputs

### Configure an IoT solution for Time Series Insights (TSI)
* Implement solutions to handle telemetry and time-stamped data
* Create an Azure Time Series Insights (TSI) environment
* Connect the IoT Hub and the Time Series Insights (TSI)

## Homework:
### [AZ-220 IoT Labs](https://microsoftlearning.github.io/AZ-220-Microsoft-Azure-IoT-Developer) 
* Module 10: Azure Security Center and IoT Security
  * [Lab 19: Detect if your IoT Device was Tampered with Azure Security Center for IoT](https://microsoftlearning.github.io/AZ-220-Microsoft-Azure-IoT-Developer/Instructions/Labs/LAB_AK_19-azure-security-center-for-iot.html)
  <br />Exercise 1: Verify Lab Prerequisites
  <br />Exercise 2: Enable Azure Security Center for IoT Hub
  <br />Exercise 3: Create and Register a New Device
  <br />Exercise 4: Create a Security Module Twin
  <br />Exercise 5: Deploy Azure Security Center for IoT C# Security Agent
  <br />Exercise 6: Configure Solution Management
  <br />Exercise 7: Introduce custom alerts
  <br />Exercise 8: Configure the Device App
  <br />Exercise 9: Review Security Center Alerts
  
## Quick Reference: Key Concepts and Terminology
* Three widely used authentication types are X.509 certificates, Trusted Platform Modules (TPM), and symmetric keys.
  * *X.509 certificate* - A type of digital identity you can use for authentication. The X.509 certificate standard is documented in IETF RFC 5280. 
  <br />In Azure IoT, there are two ways to authenticate certificates:
    * Thumbprint. A thumbprint algorithm is run on a certificate to generate a hexadecimal string. The generated string is a unique identifer for the certificate.
    * CA authentication based on a full chain. A certificate chain is a hierarchical list of all certificates needed to authenticate an end-entity (EE) certificate. To authenticate an EE certificate, it's necessary to authenticate each certificate in the chain including a trusted root CA.
    <br />**Pros for X.509**
      * X.509 is the most secure authentication type supported in Azure IoT.
      * X.509 allows a high level of control for purposes of certificate management.
      * Many vendors are available to provide X.509 based authentication solutions.
    <br />**Cons for X.509**
      * Many customers may need to rely on external vendors for their certificates.
      * Certificate management can be costly and adds to total solution cost.
      * Certificate life-cycle management can be difficult if logistics are not well thought out.
  * *Trusted Platform Module (TPM)* - A standard for securely generating and storing cryptographic keys, also known as ISO/IEC 11889. TPM also refers to a virtual or physical I/O device that interacts with modules that implement the standard. A TPM device can exist as discrete hardware, integrated hardware, a firmware-based module, or a software-based module.
    <br />There are two key differences between TPMs and symmetric keys:
      * TPM chips can also store X.509 certificates.
      * TPM attestation in DPS uses the TPM endorsement key (EK), a form of asymmetric authentication. With asymmetric authentication, a public key is used for encryption, and a separate private key is used for decryption. In contrast, symmetric keys use symmetric authentication, where the private key is used for both encryption and decryption.
    <br />**Pros for TPM**
      * TPMs are included as standard hardware on many Windows devices, with built-in support for the operating system.
      * TPM attestation is easier to secure than shared access signature (SAS) token-based symmetric key attestation.
      * You can easily expire and renew, or roll, device credentials. DPS automatically rolls the IoT Hub credentials whenever a TPM device is due for reprovisioning.
    <br />**Cons for TPM**
      * TPMs are complex and can be difficult to use.
      * Application development with TPMs is difficult unless you have a physical TPM or a quality emulator.
      * You may have to redesign the board of your device to include a TPM in the hardware.
      * If you roll the EK on a TPM, it destroys the identity of the TPM and creates a new one. Although the physical chip stays the same, it has a new identity in your IoT solution.
  * *Symmetric key* - The same key is used to encrypt and decrypt messages. As a result, the same key is known to both the device and the service that authenticates it. Azure IoT supports SAS token-based symmetric key connections. Symmetric key authentication requires significant owner responsibility to secure the keys and achieve an equal level of security with X.509 authentication. If you use symmetric keys, the recommended practice is to protect the keys by using a hardware security module (HSM).
    <br />**Pros for symmetric key**
      * Using symmetric keys is the simplest, lowest cost way to get started with authentication.
      * Using symmetric keys streamlines your process because there's nothing extra to generate.
    <br />**Cons for symmetric key**
      * Symmetric keys take a significant degree of effort to secure the keys. The same key is shared between device and cloud, which means the key must be protected in two places. In contrast, the challenge with TPM and X.509 certificates is proving possession of the public key without revealing the private key.
      * Symmetric keys make it easy to follow poor security practices. A common tendency with symmetric keys is to hard code the unencrypted keys on devices. While this practice is convenient, it leaves the keys vulnerable. You can mitigate some risk by securely storing the symmetric key on the device. However, if your priority is ultimately security rather than convenience, use X.509 certificates or TPM for authentication.   
      
## Resources
* [IoT Technical Community](https://techcommunity.microsoft.com/t5/internet-of-things-iot/ct-p/IoT)
* [Microsoft Learn IoT Learning Paths](http://aka.ms/mslearniot)
* [Azure IoT Reference Architecture Guide](https://docs.Microsoft.com/azure/architecture/reference-architectures/iot)
* [Azure IoT Security](https://azure.microsoft.com/en-in/overview/iot/security/)
* [Selecting device authentication options](https://docs.microsoft.com/en-us/azure/iot-dps/concepts-device-oem-security-practices)
* [Device Authentication using X.509 CA Certificates](https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-x509ca-overview)
* [Quickstart: Enroll X.509 devices to the Device Provisioning Service using C#](https://docs.microsoft.com/bs-latn-ba/azure/iot-dps/quick-enroll-device-x509-csharp)
* [Device provisioning: Identity attestation with TPM](https://azure.microsoft.com/en-in/blog/device-provisioning-identity-attestation-with-tpm)
* [Quickstart: Enroll TPM device to IoT Hub Device Provisioning Service using C# service SDK](https://docs.microsoft.com/bs-latn-ba/azure/iot-dps/quick-enroll-device-tpm-csharp)
* [How to provision legacy devices using symmetric keys](https://docs.microsoft.com/en-us/azure/iot-dps/how-to-legacy-device-symm-key)
* [Quickstart: Provision a simulated device with symmetric keys](https://docs.microsoft.com/en-us/azure/iot-dps/quick-create-simulated-device-symm-key)
* [How to use different attestation mechanisms with Device Provisioning Service Client SDK for C](https://docs.microsoft.com/en-us/azure/iot-dps/use-hsm-with-sdk)
* [Azure Security Center (ASC)](https://azure.microsoft.com/en-in/services/security-center/)
* [Quickstart: Onboard Azure Security Center for IoT service in IoT Hub](https://docs.microsoft.com/en-us/azure/asc-for-iot/quickstart-onboard-iot-hub)
* [Quickstart: Create an azureiotsecurity module twin](https://docs.microsoft.com/en-us/azure/asc-for-iot/quickstart-create-security-twin)
* [Quickstart: Create custom alerts](https://docs.microsoft.com/en-us/azure/asc-for-iot/quickstart-create-custom-alerts)
* [Azure Sphere](https://azure.microsoft.com/en-in/services/azure-sphere/) - Know of it, but you shouldn't need to know implementation details

NOTE: In most cases, exams do NOT cover preview features, and some features will only be
added to an exam when they are GA (General Availability).
