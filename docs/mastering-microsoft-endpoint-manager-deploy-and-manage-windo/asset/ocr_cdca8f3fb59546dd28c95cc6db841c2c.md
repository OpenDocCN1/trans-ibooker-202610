All cloud PC devices have a specific model name (contains a cloud PC) and an enrollment profile name (equates to a cloud PC provisioning profile) that can be used to filter them from an all-devices assignment:

![](bbox=[0.151, 0.176, 0.851, 0.507])

Figure 4.61 – Creating a device-based filter

# Publishing apps and scripted actions

We won't cover application delivery in depth in this chapter as we will do that in *Chapter 8, Application Management and Delivery*. However, I'd like to highlight the following supported formats:

+   - `.MSI`
+   - `.exe`
+   - `.MSIX`
+   - `.AppV`
+   - `.AppX`

If you want to provide any custom scripted actions and publish them to your cloud PC, that's possible too. You can easily publish the script action as a PowerShell script.

You can find a list of actions/scripts in this GitHub repository https://github.com/CloudManagedDesktops.