
In cases where you find Task Manager to be lacking and want to dig a little bit deeper, go ahead and launch the newer Resource Monitor. This can be invoked by using the Start menu search function, but if you have Task Manager open anyway, visit the Performance tab. Way down at the bottom, look for a link called Open Resource Monitor.

## Resource Monitor

Like Task Manager on steroids, Resource Monitor can take system monitoring and troubleshooting even further. There is plenty of the same information that we just experienced inside Task Manager but laid out in a different format. CPU, memory, and disk utilization metrics are present, as well as monitoring of your network interfaces. There are various ways to sort the items utilizing resources, quickly identifying the high hitters for CPU and memory, and there are also right-click functions to end or suspend processes. On the Overview tab, as shown in Figure 15.13, you can see that I can quickly identify the reason for my high CPU utilization – PowerShell is consuming almost 100% of available CPU resources:

![](img/d2d47a2ce8c1af61acefc2cdd6d9df70_634_0.png)

Figure 15.13: Using Resource Monitor to investigate high CPU utilization

Exploring the different tabs inside Resource Monitor will show you more detailed information about CPU, memory, and even disk resource consumption. This is particularly cool because you can see disk storage stats and available disk space right alongside metrics such as Disk Queue Length, which is an important indicator to monitor when hard disk performance may be a bottleneck.

The only tab I haven't mentioned yet is **Network**, which is my favorite place to visit inside Resource Monitor. A fairly common client/server troubleshooting task is to track down what a particular application may be calling for at a network level, and this can be a difficult thing to ascertain. For example, I have installed an application onto a client or server, and this application makes calls to another server to grab information or to interact with it in some way. Let's pretend the application isn't working correctly. First, you do some basic network troubleshooting and verify that the two machines can communicate with each other, by pinging back and forth. This means network traffic is flowing, but the application still won't connect. Your next thought is the firewall - what if the firewall on the receiving server is not allowing the traffic, or perhaps there is even a physical firewall device somewhere in-between that is restricting the ports being used by the application? You are unsure of what ports the app may be trying to use, and it's too late in the day to call the software vendor (if one exists).

Resource Monitor to the rescue! Now, if you've been around the block once or twice, you may be thinking, "Couldn't I just install Wireshark and use it to review network traffic leaving my machine?" The answer would be yes, but this information is to portray a similar capability baked right into Windows.

Inside the **Network** tab of Resource Monitor, you can see the applications and **Process IDs (PIDs)** that are currently generating network traffic on this computer or server. Keep in mind that Resource Monitor is available inside Windows Server, and it is also available inside Windows 10! Launch your application, look for the PID, and check the box next to it. Then, drop down the **TCP Connection** section, and if that application is utilizing a TCP traffic stream (or attempting to), as most applications do, you will see right here exactly what server name or IP address the application is calling for, and what ports it is using to attempt connection. The interesting columns here are **Remote Address** and **Remote Port**:

![](img/d2d47a2ce8c1af61acefc2cdd6d9df70_636_0.png)

Figure 15.14: Using Resource Monitor for network troubleshooting

It is easy to see that my Outlook.exe application is reaching out to a couple of different IP addresses, using TCP port 443. If this information were from your own application, you could now use this information to verify that the application is calling for the IP or server that you expect, and that the port being used is accessible, listening, and not being blocked on the receiving server.

## Performance Monitor

Another built-in monitoring tool is called Performance Monitor, commonly referred to as Perfmon. Almost every component inside Windows Server has predefined performance monitor counters, and Perfmon taps into those counters to display extremely in-depth information about what is happening with those components, but only when you specifically set up reporting to see it. Perfmon does not log anything by default, because to do so would consume plenty of server resources, so this tool is generally only to be used temporarily during troubleshooting or for a specific reason, and then disabled again when you are finished.

The easiest way to launch Perfmon is to Start | Run or open a Command Prompt or PowerShell, simply type Perfmon, and then press Enter. This launches the interface and by default, you can see that it has plugged in a counter for % Processor Time. You can obviously find CPU percentage information in much easier and better-looking places than Perfmon, so this is displayed simply as a sample of data:

![](img/d2d47a2ce8c1af61acefc2cdd6d9df70_637_0.png)

Figure 15.15: Performance Monitor default counter

What is very interesting here is that you can quickly see the **Average**, **Minimum**, and **Maximum** data fields. You will find that these fields exist with most counters, and this is some of the most useful information that I have pulled using Perfmon during real-life troubleshooting scenarios.

Right-click anywhere inside that graph and select **Add Counters....** Here, we can discover the incredible breadth of data counters that are available to monitor with Perfmon:

![](img/d2d47a2ce8c1af61acefc2cdd6d9df70_638_0.png)

Figure 15.16: Adding counters to Perfmon

There is no way we can cover everything here, so I'll list out a few examples of counters that are useful in most server performance troubleshooting scenarios:

- Processor > % Processor Time (this is the one added by default)
- Memory > Available MBytes
- Network Interface > Bytes Received/sec
- Network Interface > Bytes Sent/sec (and these can be further defined per-NIC)
- PhysicalDisk > % Disk Time
- PhysicalDisk > Current Disk Queue Length

Many counters are role-specific. Things like Terminal Services > Total Sessions can give you counters on how many users you have logged into your RDSH hosts. Or you can use Print Queue > Total Jobs Printed when trying to gain an idea of how busy or overworked your print server might be. In a previous job, I often utilized IP-HTTPS, DNS64, and IPsec counters to keep tabs on under the hood components within the Microsoft Remote Access technologies. I have gone ahead and added all the performance-related counters that I specified previously (there are many, many more options available), and the Perfmon data graph has become a mess of multiple colors and overlapping line graphs. While the default graph view is interesting, it's not very easy to follow. Instead, if you drop down the toolbar icon that looks like a little graph, you'll find three options for how to display this data: Line, Histogram bar, and Report. I prefer looking at numbers, but the Report view does not display the Average-Minimum-Maximum, and those are really useful. So, you may find yourself utilizing a combination of all three views on a regular basis:

![](img/d2d47a2ce8c1af61acefc2cdd6d9df70_639_0.png)

Figure 15.17: Performance metrics in the Report view

This is cool real-time information, but how often are you really digging in and troubleshooting a real-time problem with this much depth? Let's be honest; if you have a server tanking performance during working hours, you are probably going to restart the entire server to get things normalized before using Perfmon for a hardware review. Instead, what I have always found Perfmon to be most useful for is gathering statistics over a certain period. A great example is repeated reports of slowness on a particular server. Perhaps an RDSH or Remote Access server where users are interacting with it every day, and every few days, you receive support tickets indicating that the system was running slowly or wouldn't allow anyone new to log in. By the time that ticket crosses your desk and you hop into the server, whatever the problem was has disappeared, and the server is running happily again.
