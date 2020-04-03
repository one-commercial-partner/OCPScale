# Lab 13: Create a Custom WVD Master Image


[Microsoft Reference Article](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/capture-image-resource)

There are many ways to create WVD master images. This article will cover a mainly manual approach that is simple to use.

#### Task 1: Create a new Virtual Machine (VM) in Azure 

In your Azure Portal, go to create a resource, and in the search field type
“Microsoft Windows 10”

![image.png](/.attachments/image-win10ms-mp.png)

Once you pick which version you want, build the VM.

![image.png](/.attachments/image-create-a-vm-image.png)

This guide does not go into the detail of creating an Azure VM; however, please make sure you open the public inbound ports for RDP when you create it.

For more details on how to create a VM, see [https://docs.microsoft.com/en-us/azure/virtual-machines/windows/quick-create-portal](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/quick-create-portal).

Once the VM deployment is complete, login to the VM over RDP with the credentials you supplied when creating the VM.

#### Task 2: Run Windows Update

Despite the Azure support teams best efforts, the Marketplace images are always completely up to date. The best and most secure practice is to keep you master image as up to date as possible. Perform the manual steps to run Windows Update from the Settings app until no more updates are required. Reboot as necessary.

![image.png](/.attachments/image-windows-update.png)

The Windows Update Settings dialog should look like this at the end:

![image.png](/.attachments/image-up-to-date.png)

#### Task 3: Run the Automated Prepare WVD Image Script 

The devs for this content decided to develop a script (Prepare-WVDImage.ps1) to automate the baseline image build tasks to allow you to quickly create a custom image that incorporates Microsoft's main business applications and configures policies and settings to optimize the user experience. The script is parameterized to allow the image builder to selectively perform the following functions.

1. Installs the *latest* version of Microsoft Office 365 monthly channel and apply the settings as specified by [https://docs.microsoft.com/en-us/azure/virtual-desktop/install-office-on-wvd-master-image
](https://docs.microsoft.com/en-us/azure/virtual-desktop/install-office-on-wvd-master-image).

2. Installs the *latest* OneDrive *per-machine* and apply settings as specified by [https://docs.microsoft.com/en-us/azure/virtual-desktop/install-office-on-wvd-master-image
](https://docs.microsoft.com/en-us/azure/virtual-desktop/install-office-on-wvd-master-image).

3. Installs Microsoft Teams *per-machine* as specified by [https://docs.microsoft.com/en-us/azure/virtual-desktop/teams-on-wvd](https://docs.microsoft.com/en-us/azure/virtual-desktop/teams-on-wvd).

4. Installs the *latest* production FSLogix Agent and apply settings as specified by [https://docs.microsoft.com/en-us/fslogix/install-ht](https://docs.microsoft.com/en-us/fslogix/install-ht).

5. Installs Microsoft Edge Enterprise and apply settings as discussed in [https://docs.microsoft.com/en-us/deployedge/deploy-edge-with-configuration-manager](https://docs.microsoft.com/en-us/deployedge/deploy-edge-with-configuration-manager).

6. Applies the WVD Image Settings as specified in [https://docs.microsoft.com/en-us/azure/virtual-desktop/set-up-customize-master-image](https://docs.microsoft.com/en-us/azure/virtual-desktop/set-up-customize-master-image).

7. Applies the Azure VM Settings as specified in [https://docs.microsoft.com/en-us/azure/virtual-machines/windows/prepare-for-upload-vhd-image](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/prepare-for-upload-vhd-image).

8. Performs an automated Disk Cleanup using the built-in Windows Disk Cleanup Wizard utility (cleanmgr.exe). ![Note] If you do not disable this action, you will want to reboot after running the script.

The benefits of using the script are that you automatically get an *optimized* install of the listed software and base Windows 10 configuration for Windows Virtual Desktop. The script is documented within starting with the purpose of the parameters and generates a log in "c:\windows\logs\imageprep" for troubleshooting. Where possible, the script applies the settings and configuration to the Local Group Policy on the system via the lgpo tool from the Microsoft Security Compliance Toolkit [https://www.microsoft.com/en-us/download/details.aspx?id=55319](https://www.microsoft.com/en-us/download/details.aspx?id=55319). In addition, the local group policy is documented in an html file and a backup both located in "C:\windows\logs\imageprep\lgpo". This approach is taken to simplify troubleshooting via Group Policy Results versus having to search for registry settings directly.

The script and tools are maintained in the WVD Lab Sources Repo under the WVDImaging\\Manual folder located [here](https://servicescode.visualstudio.com/WVD%20Bootcamp%20Labs/_git/WVDSources?path=%2FWVDImaging%2FManual). The contents of the Manual folder should be downloaded as a .zip file directly using this [link](https://servicescode.visualstudio.com/45c551a2-105f-4bdb-b096-134ebc8001c5/_apis/git/repositories/ec513ac5-8907-4014-bcb7-a84229a2d7be/items?path=%2FWVDImaging%2FManual&versionDescriptor%5BversionOptions%5D=0&versionDescriptor%5BversionType%5D=0&versionDescriptor%5Bversion%5D=master&resolveLfs=true&%24format=zip&api-version=5.0&download=true).

1. Download the zip file to your workstation and copy and paste the zip through RDP to the desktop of the master image VM. If you are prompted to login by this download, use your @microsoft.com account.

2. On the master image VM, right click on the zip file on your desktop and select **Extract All...**.

3. Extract the files to "c:\BuildArtifacts".

   ![image.png](/.attachments/image-extract-buildartifacts.png)

4. Open 'c:\buildartifacts\manual\Prepare-WVDImage.ps1' in the Powershell Integrated Scripting Environment (Powershell ISE) and update the parameters as necessary based on the documentation within the script. Once you are happy, with the script configuration, save it.

   ![image.png](/.attachments/image-edit-script.png)

5. Open Powershell administratively.

6. Navigate to "C:\BuildArtifacts\Manual".

7. Run **Set-ExecutionPolicy -ExecutionPolicy Bypass -Scope Process** and select **[Y] Yes**.

   ![image.png](/.attachments/image-powershell-setexecutionpolicy.png)

8. Execute the script by typing **.\prepare-wvdimage.ps1** and hitting <Enter>. This script will produce output to the console and the log located in "c:\windows\logs\imageprep". It may take some time to run especially if you selected to install Office 365 as it will download those files dynamically from the Microsoft Content Distribution Network (CDN).

   If you selected to install office, you will see a setup.exe popup for sometime. Do not close this.

   ![image.png](/.attachments/image-running-script1.png)

   If you selected to install OneDrive, you will see the onedrive popup.

   ![image.png](/.attachments/image-script-onedriveinstall.png)

   If you selected to cleanupimage, you will see the Disk Cleanup wizard running and it may stay on the "Windows Update Cleanup" task for a few minutes while it cleans out older files in the Windows Side by Side store.

   ![image.png](/.attachments/image-cleanmgr-running.png)

   You may notice after the Disk Cleanup Wizard disappears from the screen that it appears that the powershell window is frozen. It does take a few minutes for the cleanmgr.exe process to close. You can select the powershell window and continue to hit the up arrow on your keyboard until you are presented with an active prompt. At this point, you can delete the c:\buildartifacts directory and the files on the desktop. Empty the recycle bin afterwards.

   If you are interested in the LGPO backup and settings, navigate to C:\Windows\Logs\ImagePrep\LGPO.

   ![image.png](/.attachments/image-lgpodirectory.png)

9. Reboot the master image VM.

#### Task 4: Run Sysprep

1. Reconnnect and logon to the VM. 

2. Open an administrative command prompt.

2. Navigate to **c:\windows\system32\sysprep**.

3. Run **sysprep.exe /oobe /generalize /shutdown**.

   ![image.png](/.attachments/image-sysprep.png)

   The system will automatically shutdown and disconnect your RDP session.

#### Task 5: Create a managed image from the Master Image VM

1. Go into the Azure Portal and find the VM that you ran sysprep on, The **Status** should be listed as "Stopped". Select **Stop** to move it to a stopped state (deallocated)

   ![image.png](/.attachments/image-stop-vm.png)

2. Once this is done, on the top of the same screen, click capture.

   ![image.png](/.attachments/image-capture-image.png)

3. Once you click capture, fill in all the associated fields.

   ![image.png](/.attachments/image-createimage-dialog.png)

   Once completed you should now see the image,

   ![image.png](/.attachments/image-imageproperties.png)

4. Record the image name and resource group in order to use it as a custom image in the next task.

#### Task 6: Provision a Host Pool with the custom image 

To start provisioning a host pool with the custom image, follow the instructions in [Exercise 6](Exercise-6%3A-Deploy-a-Pooled-Host-Pool.md) except for the Virtual Machine Settings screen. Instead of selecting a 'Gallery' Image, you will select a 'Managed Image' as in the screenshot below.

![image.png](/.attachments/image-provisionhostpool-2.png)

Complete the remainder of the instructions in [Exercise 6](Exercise-6%3A-Deploy-a-Pooled-Host-Pool.md) to provision a new host pool with the managed image.