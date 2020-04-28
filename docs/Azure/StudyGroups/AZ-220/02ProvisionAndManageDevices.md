# AZ-220 Provision and manage devices (20-25%)

* [AZ-220: Microsoft Azure IoT Developer Exam](https://docs.microsoft.com/en-us/learn/certifications/exams/az-220)
* [Microsoft Certified: Azure IoT Developer Specialty](https://docs.microsoft.com/en-us/learn/certifications/azure-iot-developer-specialty)
* [Yammer Azure Study Group](http://aka.ms/azurecsg)

## Skills Measured:
### Implement the Device Provisioning Service (DPS)
* Create a Device Provisioning Service
* Create a new enrollment in DPS
* Manage allocation policies by using Azure Functions
* Link an IoT Hub to the DPS

### Manage the device lifecycle
* Provision a device by using DPS
* Deprovision an autoenrollment
* Decommission (disenroll) a device

### Manage IoT devices by using IoT Hub
* Manage devices list in the IoT Hub device registry
* Modify device twin tags and properties
* Trigger an action on a set of devices by using IoT Hub Jobs and Direct Methods
* Set up Automatic Device Management of IoT devices at scale

### Build a solution by using IoT Central
* Define a device type in Azure IoT Central
* Configure rules and actions in Azure IoT Central
* Define the operator view
* Add and manage devices from IoT Central
* Monitor devices

## Homework:
### [AZ-220 IoT Labs](https://microsoftlearning.github.io/AZ-220-Microsoft-Azure-IoT-Developer) 
* Module 3: Device Provisioning at Scale - [Lab 05: Individual Enrollment of a Device in DPS](https://microsoftlearning.github.io/AZ-220-Microsoft-Azure-IoT-Developer/Instructions/Labs/LAB_AK_05-individual-enrollment-of-device-in-dps.html)
<br />Exercise 1: Verify Lab Prerequisites
<br />Exercise 2: Create new individual enrollment (Symmetric keys) in DPS
<br />Exercise 3: Configure Simulated Device
<br />Exercise 4: Test the Simulated Device
<br />Exercise 5: Retire the Device

* Module 3: Device Provisioning at Scale - [Lab 06: Automatically provision IoT devices securely and at scale with DPS](https://microsoftlearning.github.io/AZ-220-Microsoft-Azure-IoT-Developer/Instructions/Labs/LAB_AK_06-automatic-enrollment-of-devices-in-dps.html)
<br />Exercise 1: Verify Lab Prerequisites
<br />Exercise 2: Generate and Configure X.509 CA Certificates using OpenSSL
<br />Exercise 3: Create Group Enrollment (X.509 Certificate) in DPS
<br />Exercise 4: Configure simulated device with X.509 certificate
<br />Exercise 5: Handle device twin desired property Changes
<br />Exercise 6: Test the Simulated Device
<br />Exercise 7: Retire Group Enrollment

* Module 8: Device Management - [Lab 15: Remotely monitor and control devices with Azure IoT Hub](https://microsoftlearning.github.io/AZ-220-Microsoft-Azure-IoT-Developer/Instructions/Labs/LAB_AK_15-remotely-monitor-and-control-devices.html)
<br />Exercise 1: Verify Lab Prerequisites
<br />Exercise 2: Write Code to Send and Receive Telemetry
<br />Exercise 3: Create a Second App to Receive Telemetry
<br />Exercise 4: Write Code to Invoke a Direct Method
<br />Exercise 5: Write Code for Device Twins

* Module 8: Device Management - [Lab 16: Automate IoT Device Management with Azure IoT Hub](https://microsoftlearning.github.io/AZ-220-Microsoft-Azure-IoT-Developer/Instructions/Labs/LAB_AK_16-automatic-device-management.html)
<br />Exercise 1: Verify Lab Prerequisites
<br />Exercise 2: Write code to simulate device that implements firmware update
<br />Exercise 3: Test firmware update on a single device

* Module 11: Build with IoT Central - [Lab 20: Build with IoT Central](https://microsoftlearning.github.io/AZ-220-Microsoft-Azure-IoT-Developer/Instructions/Labs/LAB_AK_20-build-with-iot-central.html)
<br />Exercise 1: Create and Configure Azure IoT Central
<br />Exercise 3: Monitor a Simulated Device
<br />Exercise 4: Create a free Azure Maps account
<br />Exercise 5: Create a Programming Project for a Real Device
<br />Exercise 6: Test Your IoT Central Device
<br />Exercise 7: Create multiple devices

### Sign up for [Online Workshop Series: Build End-to-End IoT Solutions](https://aka.ms/IoT-online-workshop)
* Device provisioning at scale - April 30th

## Device Provisioning Service (DPS) Terminology and Key Concepts
* Service Operations Endpoint – Used for managing DPS and the enrollment list
* Device Provisioning Endpoint – Single address used for all provisioning, shared across all customers and DPS instances
* Linked IoT Hubs – Target Azure IoT Hub instances for the DPS
* Allocation Policy – As previously mentioned, the mapping of device to target Azure IoT Hub
* Enrollment – The record of a device or group of devices that may register against the DPS
* Registration – The record of a successful registration/provisioning of a device
* Operations – The billing unit for DPS; one successfully completed request
* Device Enrollment Concepts:
<br />ID scope – Differentiates various DPS instances and tenants at the fixed, shared target endpoints
<br />Registration ID – Uniquely identifies a device in the DPS instance
<br />Device ID – Uniquely identifies a device in the associated IoT Hub instance
<br />Attestation mechanism – the way a device proves its identity to the DPS
<br />- X.509 Certificates
<br />- TPM nonce challenge
<br />- Symmetric key
* Device Enrollment Types
<br />Individual Enrollments - An Individual enrollment is an entry for a single device that may register. Individual enrollments may use either X.509 certificates or SAS tokens (from a physical or virtual TPM) as attestation mechanisms. 
<br />Group Enrollments - An Enrollment group is an entry for a group of devices that share a common attestation mechanism of X.509 certificates, signed by the same signing certificate, which can be the root certificate or the intermediate certificate, used to produce device certificate on physical device.
* Security Concepts: Certificates
<br />X.509 Certificates – Digital identity based on private/public key pairs and a chain of trust
<br />Issued by a certificate authority (CA)
<br />Certificate rules for DPS
<br />- Chain must be trusted
<br />- Group or individual enrollment
<br />- Individual overrides group
* Security Concepts: Hardware
<br />Hardware security module (HSM) – used for secure, hardware-based storage of device secrets
<br />Trusted Platform Module (TPM) – a specification for storing keys or the interface for communicating with an HSM acting as a TPM
<br />Two hardware keys for the TPM
<br />- Endorsement key (EK) – unique identifier for the TPM; read-only, injected by the manufacturer
<br />- Storage root key (SRK) – protects the TPM secrets; generated when a user takes ownership of the TPM


## Resources
* [IoT Hub Documentation](https://docs.microsoft.com/en-us/azure/iot-hub/)
* [IoT Technical Community](https://techcommunity.microsoft.com/t5/internet-of-things-iot/ct-p/IoT)
* [“Building IoT Solutions with Azure” Developer Guide](https://discover.Microsoft.com/azure-iot-building-solutions-dev-guide)
* [Microsoft Learn IoT Learning Paths](http://aka.ms/mslearniot)
* [Azure IoT Reference Architecture Guide](https://docs.Microsoft.com/azure/architecture/reference-architectures/iot)
* [What is IoT Hub Device Provisioning Service?](https://docs.microsoft.com/en-us/azure/iot-dps/about-iot-dps)
* [How to deprovision devices that were previously auto-provisioned](https://docs.microsoft.com/en-us/azure/iot-dps/how-to-unprovision-devices)


NOTE: In most cases, exams do NOT cover preview features, and some features will only be
added to an exam when they are GA (General Availability).
