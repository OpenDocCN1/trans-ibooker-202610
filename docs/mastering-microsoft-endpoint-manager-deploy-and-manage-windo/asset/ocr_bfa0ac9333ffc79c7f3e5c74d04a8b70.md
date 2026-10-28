The same applies to language settings. Pick the OS default settings when you want to drive language settings from within the Windows OS, or overwrite them when you want to separate the browser language:

![](bbox=[0.137, 0.174, 0.838, 0.536])

Figure 8.63 – Microsoft Edge for Windows 10 Language menu

Microsoft Edge is part of every new release of Windows, so this app type will be redundant sometime in the future.

# What is MSIX?

MSIX is a Windows app package format that provides a modern packaging experience to all Win32, UWP, and Windows apps. It's a new way of doing application virtualization compared to technologies such as App-V.

Separating the applications from the images to update and assign applications without doing an image update sounds like a solution, right? Well, that's exactly what MSIX can accomplish. Let me explain how you could package an application here:

+   1. Declarative install via the manifest file.
+   2. The app signature needs to be trusted on the device.
+   3. Tamper protection via BlockMap and signature.