> **Important Note**
> If **Windows Defender Application Control (WDAC)** is configured with the reputation of the app determined by Microsoft’s Intelligent Security Graph, Autopilot Reset might fail.

Windows Autopilot Reset has a great use case for a device reset if the same user needs to reuse the devices again, where other options are available in other scenarios. In the next section, we will cover wiping and resetting.

# Wiping and resetting your devices

A factory reset returns the device to its default settings. This removes all personal and company data and settings from this device. You can choose whether to keep the device enrolled and the user account associated with this device. You cannot revert this action. Are you sure you want to reset this device? Here are the options available to you:

+   - **Wipe device, but keep enrollment state and associated user account.**
+   - **Wipe device, and continue to wipe even if device loses power. If you select this option, please be aware that it might prevent some Windows 10 devices from starting up again..**

This option makes sure that the wipe action can't be circumvented by turning off the device. This option will keep trying to reset the device until successful. In some configurations, this action may leave the device unable to reboot.

This issue may be caused when the installation of Windows has major corruption that is preventing the OS from reinstalling. In such a case, the process fails and leaves the system in the Windows Recovery Environment:

**Wipe - APTEST01**
Factory reset returns the device to its default settings. This removes all personal and company data and settings from this device. You can choose whether to keep the device enrolled and the user account associated with this device. You cannot revert this action. Are you sure you want to reset this device?
☐ Wipe device, but keep enrollment state and associated user account
☐ Wipe device, and continue to wipe even if device loses power. If you select this option, please be aware that it might prevent some Windows 10 devices from starting up again.
[Yes] [No]

Figure 7.41 – Windows device wipe