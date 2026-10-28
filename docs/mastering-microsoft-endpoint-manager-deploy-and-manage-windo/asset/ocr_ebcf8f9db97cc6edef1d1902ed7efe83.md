A great example is the **Application control** policy type.

The **Application control** policy can help mitigate security threats. It does this by restricting the applications that users are allowed to run and the code that runs in the system's core (kernel). Application control policies can also block unsigned scripts and Windows Installer (MSI) and restrict Windows PowerShell to run in Constrained Language mode.

With a few settings, you can easily harden your Windows operating system:

![](bbox=[0.13, 0.265, 0.82, 0.58])

Figure 10.8 – Microsoft Defender Application control policy

Now, let's enable Windows SmartScreen (not only for Microsoft Edge, as we will show later in this chapter).

Set **App locker application control** to **Enforce Components, Store Apps, and Smartlocker** – this will enable **Windows Defender Application Control (WDAC)** so that only the following can be executed on the Windows operating system:

+   - Windows components
+   - Third-party hardware and software kernel drivers
+   - Microsoft Store-signed apps
+   - [Optional] Reputable apps, as defined by the **Intelligent Security Graph (ISG)**

WDAC policies apply to the managed computer as a whole and affect all users of the device.