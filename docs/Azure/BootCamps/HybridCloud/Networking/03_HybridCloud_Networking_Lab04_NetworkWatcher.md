
# Lab 4 - Network Watcher

In this lab you are going to  test network communication with Network Watcher. First, enable a network watcher in at least one Azure region, and then use Network Watcher's IP flow verify capability.

## Enable network watcher in your Region

1. In the portal, select **All services**. In the Filter box, enter **Network Watcher**. When Network Watcher appears in the results, select it.
2. Enable network watcher by clicking on the elipsys **...** and then choosing **Enable network watcher in a...**

   >Network Watcher should be enabled in 33 regions.

## Packet Capture and examination

### Task 1 - Enable Packet Capture

>Make sure LBVM1 is running before you begin.

1. On the **Network Watcher** blade, under **Network diagnostic tools**, select **Packet Capture**, then **+Add**.
2. Enter the following and click **Save**:
   - Resource Group: **LoadBalVMs**
   - Target VM: **LBVM1**
   - Packet Capture Name: **Packets**

>Note that it may take several minutes for the packet capture to initialize.  Do not continue until the task is complete.  In the event you receive an error, ensure that LBVM1 is running.

### Task 2 - Capture Packets

1. Find the public IP address for the load balancer **LB01** which should be named **LBPublicIP**.  Copy and paste the public IP address of your Load Balancer into the address bar of your browser. Hit refresh and notice the default page of the IIS web server is displayed in the browser.
2. Stop **LBVM2**. Hit refresh and notice the default page of the IIS web server is displayed in the browser.
3. Stop the packet capture by returning to **Network Watcher** > **Packet Capture** >  **Packets** >**Right-Click** > **Stop**.
4. Scroll down and click on the .cap file under **Storage location** near the bottom of the screen.  On the following pane click on **Download**  and download the file to your **Downloads** folder on your local computer.
5. Install network monitor from the following: <https://www.microsoft.com/en-US/download/details.aspx?id=4865>
6. Launch Network Monitor and open the capture file from your **Downloads** folder.  Examine the contents of your packet capture session.
