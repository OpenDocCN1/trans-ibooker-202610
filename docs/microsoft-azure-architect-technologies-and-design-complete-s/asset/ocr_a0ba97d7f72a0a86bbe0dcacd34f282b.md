features: Update Management, Inventory, and Change Tracking. Each of those features places their logs into a Log Analytics workspace, which was introduced in Chapter 1 and covered in more detail in Chapter 9. Update management was introduced in Chapter 4 and is useful for scheduling and tracking the installation of Microsoft Windows updates, aka KB patches. Many of those patches contain fixes for discovered security vulnerabilities, and having the record of the patch and when it was installed is a useful compliancy element. As just mentioned, the logs are written to Log Analytics and referenced from Azure Monitor.

# **FIGURE 6.13**  Compute & Apps Security Center overview blade

![](bbox=[0.14, 0.288, 0.848, 0.502])

Inventory and Change tracking, see Figure 6.14, allow you to monitor activities on the VM that modify the configuration of the machine in some way. After enabling Inventory and Change tracking a baseline including state of the software, the Windows registry keys, the Windows services, and files on the computer are set. Then future assessments are able to call out whenever one or more of those elements have been modified.

# **FIGURE 6.14**  Azure VM change tracking overview blade

![](bbox=[0.14, 0.685, 0.848, 0.839])