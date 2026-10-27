The key point here is that as a security professional responsible for hardening a system, you must get a listing of services running on the system and then evaluate whether each service is needed. If a service is not needed on a single machine, then disable the service through the Services console in Windows; to disable services for many machines, you could centrally disable them in an Active Directory domain using group policy. To view a list of services in Windows, follow these steps:

+   1. On the Windows system, choose Start | Administrative Tools | Services.
+   2. Right-click the service you wish to stop and then choose Stop.
+   3. To ensure that the service does not automatically start the next time Windows boots, you must change the Startup Type setting to Disabled. Right-click the service and choose Properties. This opens a Properties dialog box.
+   4. Change the Startup Type setting to Disabled, as shown in Figure 6-3.
+   5. Click OK.

# FIGURE 6-3

Modify the service to not start automatically on reboot.

![](bbox=[0.35, 0.435, 0.825, 0.862])