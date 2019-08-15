# Lab 4 - Network Watcher
To test network communication with Network Watcher, first enable a network watcher in at least one Azure region, and then use Network Watcher's IP flow verify capability.
## Enable network watcher
1)	In the portal, select **All services**. In the Filter box, enter **Network Watcher**. When Network Watcher appears in the results, select it.
2)	Enable a network watcher in the region where you deployed your VMs. Select Regions, to expand it, and then select ... to the right of **East US**.
3)	Select **Enable Network Watcher**.

## Packet Capture and examination
1)	From the left-hand pane, choose **Network Watcher**.  Under **Network diagnostic tools**, select **Packet Capture**, the **Add**.
2)	Enter the following and click **Ok**:
    * Resource Group: LBGR
    * Target VM: LBVM1
    * Packet Capture Name: Packets
    * Capture configuration: Storage Account
    * Storage Accounts:  Select the storage account previously provisioned
    * Cut and paste the default values into the three configuration boxes.
    
    *Note that it may take several minutes for the packet capture to initialize*
3)	Find the public IP address for the load balancer LBPublicIP.
4)	Shutdown LBVM2 and then paste the public IP address of your Load Balancer into the address bar of your browser. Hit refresh and notice the default page of IIS web server is displayed in the browser. 
5)	Stop the packet capture by returning to **Network Watcher** > **Packet Capture** >**Right-Click** > **Stop**.
6)	Click on the .cap file it will display the blob properties.  Download the file to your documents folder on your local computer.
7)	Install network monitor from the following: https://www.microsoft.com/en-US/download/details.aspx?id=4865 
8)	Launch Network Monitor and open the capture file from your Documents folder.  Examine the contents of your packet capture session.
