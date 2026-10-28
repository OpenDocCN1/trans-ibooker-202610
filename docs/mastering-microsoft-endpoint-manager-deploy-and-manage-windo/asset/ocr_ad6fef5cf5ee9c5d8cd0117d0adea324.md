Support for expediting an update policy is available on Windows 10 devices that are still receiving updates through Windows Update and have not reached the end of service. Devices need to be hybrid AAD-joined or AAD-joined for the policy to work.

Expedited updates use **Windows Push Notification Services (WNS)** and push notification channels to deliver the message to devices that there's an expedited update to install. This is done to speed up installation. This process enables devices to start the download and install from Windows Update as an expedited update as soon as possible, without having to wait for the device to go through the normal scheduled check-in process for talking to the Windows Update backend service.

To configure a quality update profile, do the following:

+   1. Sign in to the Microsoft Endpoint Manager admin center (https://endpoint.microsoft.com).
+   2. Select **Devices | Windows | Windows 10 Quality Updates | Create profile:**

![](bbox=[0.13, 0.41, 0.86, 0.7])

Figure 6.30 – Windows 10 quality update settings