
# Lab 4 - Network Watcher

To test network communication with Network Watcher, first enable a network watcher in at least one Azure region, and then use Network Watcher's IP flow verify capability.

## Enable network watcher

1. In the portal, select **All services**. In the Filter box, enter **Network Watcher**. When Network Watcher appears in the results, select it.
1. Enable network watcher by clicking on the elipsys **...**.

## Packet Capture and examination

1. From **Network Watcher** under **Network diagnostic tools**, select **Packet Capture**, then **Add**.
2. Enter the following and click **Ok**:
   - Resource Group: **LoadBalVMs**
   - Target VM: LBVM1
   - Packet Capture Name: **Packets**
   - Maximum bytes per packet: **0**
   - Maximum bytes per session: **1073741824**
   - Time limit: **18000**

*Note that it may take several minutes for the packet capture to initialize.  In you get an error ensure that LBVM1 is running.*

1. Find the public IP address for the load balancer LBPublicIP.
2. Shutdown LBVM2 and then paste the public IP address of your Load Balancer into the address bar of your browser. Hit refresh and notice the default page of IIS web server is displayed in the browser.
3. Stop the packet capture by returning to **Network Watcher** > **Packet Capture** >**Right-Click** > **Stop**.
4. Click on the .cap file it will display the blob properties.  Download the file to your documents folder on your local computer.
5. Install network monitor from the following: <https://www.microsoft.com/en-US/download/details.aspx?id=4865>
6. Launch Network Monitor and open the capture file from your Documents folder.  Examine the contents of your packet capture session.
