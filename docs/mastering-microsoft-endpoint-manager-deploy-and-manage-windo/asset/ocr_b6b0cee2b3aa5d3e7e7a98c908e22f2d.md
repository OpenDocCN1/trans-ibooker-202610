# Testing company domain CNAME registration for Windows enrollment

Configuring a CNAME in your DNS saves your users from having to enter the address of the **Mobile Device Management (MDM)** server when enrolling their Windows devices.

After configuring the CNAME resource records in your DNS, enter the corresponding domain to confirm that it has been configured correctly.

Changes to DNS records might take up to 72 hours to propagate. You can find the configuration option that makes this possible in the following screenshot:

![](bbox=[0.339, 0.312, 0.663, 0.673])

Figure 6.2 – CNAME validation setup

Let's now switch over to the **Enrollment Status Page** screen to learn more about the results and status of your enrollment into Microsoft Endpoint Manager.

# Enrollment Status Page

The **Enrollment Status Page** screen displays the provisioning progress after a new device is enrolled, as well as when new users sign in to the device. This enables IT administrators to optionally prevent (block) access to the device until it has been fully provisioned, while at the same time giving users information about the tasks remaining in the provisioning process.