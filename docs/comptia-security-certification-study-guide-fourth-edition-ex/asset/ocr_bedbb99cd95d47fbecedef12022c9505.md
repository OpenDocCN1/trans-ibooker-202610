+   6. Turn off Windows Firewall for the Private Network Settings, but keep it enabled for both the Domain Network Settings and the Public Network Settings. Click OK.
+   7. To allow RDP traffic to pass through your firewall, choose the Allow an App or Feature Through Windows Firewall link on the left side.
+   8. Scroll down in the list and locate Remote Desktop. Enable the check box for Remote Desktop and then ensure the check boxes for Domain and Public are selected on the right.
+   9. Click OK.
+   10. To enable logging of dropped packets, choose the Advanced Settings link on the left. The Windows Firewall with Advanced Security dialog box appears.
+   11. Right-click the Windows Firewall with Advanced Security node found on the top-left corner of the dialog box and then choose Properties.
+   12. At the bottom of the screen, click the Customize button in the Logging section.
+   13. Set the Log Dropped Packets option to Yes.

![](bbox=[0.31, 0.43, 0.73, 0.67])

+   14. Click OK. Note that you made this change on the Domain Profile tab and you would then need to do the same thing with the Public Profile tab if you wanted to log packets when on a public network.
+   15. Close all windows.