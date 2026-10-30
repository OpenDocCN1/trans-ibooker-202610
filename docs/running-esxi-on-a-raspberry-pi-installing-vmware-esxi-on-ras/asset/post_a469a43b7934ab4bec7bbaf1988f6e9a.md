
## Post-installation Configuration: VMware Host Console

Having accurate time on your ESXi is critical.

You use the web-based VMware Host Client to configure the time on the ESXi host. As I have found that the time on the Pi is usually months or even years from the actual time, I first manually set it and then turn on the NTP service to keep the time accurate on the Pi.

1. In a web browser, enter the IP address of your ESXi system prefixed with `https://`. This will bring you to the VMware Host Client.

2. Enter *root* in the **User name** text box and the password that you set when you installed ESXi in the **Password** text box and click **Log in** (see Figure 5-27).

![](img/430161835ae267467687add4a1cdc429_112_0.png)

*Figure 5-27. Log In to the VMware Host Console*

3. On the home screen, verify that you are running the ESXi on ARM Fling on a Raspberry Pi (see Figure 5-28).

![](img/430161835ae267467687add4a1cdc429_113_0.png)

*Figure 5-28. VMware Host Console Home Page*

4. From the Navigator pane, select **Manage**, select the **System** tab, and select **Time & date** (see Figure 5-29).

![](img/430161835ae267467687add4a1cdc429_114_0.png)

*Figure 5-29. Time and Date Settings*

5. Click **Edit NTP Settings**.

6. Select **Manually configure the date and time on this host**, set the date to the current UTC time, and click **Save** (see Figure 5-30).

![](img/430161835ae267467687add4a1cdc429_115_0.png)

*Figure 5-30. Manual Time Settings*

7. Click **Edit NTP Settings**.

8. Select **Use Network Time Protocol**, select **Start and stop with host**, enter `0.pool.ntp.org`, `1.pool.ntp.org`, and click **Save** (see Figure 5-31).

## Summary

There are many more steps involved when installing ESXi on an Arm platform than on an x64 system that only requires you to boot off the install medium and install it.

It may take you an hour or so the first time you install ESXi on an Arm system, but if you follow the preceding instructions, it should go smoothly. In the next chapters, we will work more with the VMware Host Console and add storage and VMs to the ESXi host.

# CHAPTER 6

## VMware Host Client

In the previous chapter, you were introduced to the VMware Host Client. In this chapter, we will take a deeper look at it. This chapter is short but important, as much of the work that you will be doing with your ESXi host from now on will involve using either the VMware Host Client or the vSphere Client. The purpose of this chapter is to give you a high-level overview of what the client is capable of and not to do actual tasks, as we will cover them in subsequent chapters.

## Overview of the Client

The VMware Host Client was originally released as a VMware Fling known as *ESXi Embedded Host Client*. When vSphere 6.5 U2 was released, the VMware Host Client was incorporated into ESXi. The client allows an HTML5 web browser to be used to view and manage an individual ESXi host. If you want or need to manage multiple ESXi hosts, you will need to use vCenter Server and the vSphere Client.

As the VMware Host Client is web-based, it can be used on almost any OS: Windows, OS X, Android, or any other that supports an HTML5 browser. The ESXi host has an embedded web server on it, so all that is required is to point a web browser to the IP address of the ESXi host and then log in to it (see Figure 6-1). To access the host, enter *root* for the username and the password that you set when you installed ESXi.

![](img/430161835ae267467687add4a1cdc429_120_0.png)

*Figure 6-1. VMware Host Client Login*

## Managing ESXi Using the Host Client

After logging in, you will be presented with a dashboard divided into different sections. The Navigator at the left of the screen allows you to customize what is shown on the main dashboard. The Recent tasks pane is at the bottom of the main dashboard. The main dashboard itself provides a summary of the system. If you click **Actions**, you will see a list of actions you can perform on the ESXi host, such as rebooting it and creating/registering VMs.

## Managing ESXi and VMs

Basic management functions can be accomplished using the client. To see what management functions are available, click **Manage** within the Navigator pane.

## Changing Default Timeout

By default, the timeout for the client is 900 seconds (15 minutes). As my client resides in a secure environment, I set the timeout to the maximum duration allowed (120 minutes). If you would like to do this, select the **System** tab and then **Advanced settings**. Enter *SessionTimeout* in the search box, and select **UserVars.HostClient.SessionTimeout**. Select the **Edit** option and enter *7200* (i.e., 120 minutes), and then click **Save** (see Figure 6-2).

![](img/430161835ae267467687add4a1cdc429_122_0.png)

## Figure 6-2. Session Timeout

There are many other values under the Advanced settings tab, but it is best not to change these unless you are explicitly instructed to do so, as serious and unexpected results may occur.

## Starting VMs Automatically

One of the settings that I have found useful is to have VMs start up automatically when the ESXi server starts. This comes in handy when the ESXi goes down due to a power outage or other unexpected events, especially for VMs that host your critical applications like your Active Directory (AD) services.

To have a VM automatically start, select the **System** tab, and click **Autostart**. Figure 6-3 will look different from what you will see on your system at this point, as you haven’t created any VMs yet, but this screenshot shows how you can enable autostart for a VM and then specify a delay in the startup and shutdown times for it. These times are useful when you want one VM that is dependent on another to start before or after the other. A prime example of this is having a VM with a database start before a VM that hosts an application that depends on the database.

*Figure 6-3. Autostart*

## ESXi Host Hardware

The Hardware tab is currently empty as Arm is not yet fully supported. Figure 6-4 shows the PCI devices on an x64 ESXi server.

*Figure 6-4. x64 Hardware*

## ESXi License Status

By clicking the **Licensing** tab, you can verify that your key is all zeroes and examine when your license will expire; this should be 180 days from when you installed it. This will also show you which features are licensed for your ESXi server. You can assign a license to your system here as well (see Figure 6-5).

![](img/430161835ae267467687add4a1cdc429_124_0.png)

*Figure 6-5. Licenses*

## ESXi Services

By clicking the **Services** tab, you will see the *ntpd* service running and that it is configured to start and stop with the host if you followed the steps listed in the previous chapter (see Figure 6-6).

## Figure 6-6. Services

## ESXi Users

You can add additional local users and allow Active Directory (AD) users to access the client. To do this, click the **Security & users** tab and then click **Users** (Figure 6-7).

## Figure 6-7. Local Users

If you have an AD domain, you can allow users access to the ESXi host by selecting **Authentication** and **Join domain** (see Figure 6-8).

![](img/430161835ae267467687add4a1cdc429_126_0.png)

***Figure 6-8.*** *Join Domain*

## Monitoring ESXi Using the Host Client

The host client monitoring features are rudimentary when compared to the vSphere Client and some third-party tools; however, these tools will give you some insight into what is happening on your host.

The host client allows you to see the resource usage of the host and the VMs that reside on it. The performance information is only stored for an hour. To find this information, click **Monitor** in the **Navigator** pane, and then select the **Performance** tab. From the drop-down menu, select **CPU**, **Memory**, **Network**, and then **Disk**.

These values will be low or nonexistent as only ESXi is running, and we have yet to add any storage or VMs to it. Figure 6-9 shows that ESXi only uses about 1.3 GB of RAM, leaving the rest for VMs. On a Pi with 8 GB of RAM, this leaves 6.6 GB of RAM; on a Pi with 4 GB, this only leaves 2.6 GB of RAM.
