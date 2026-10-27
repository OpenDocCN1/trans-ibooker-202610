# Importing the Template into the Local System

+   9. To import the template into your local system, start up the Local Security Policy Console from the Administrative Tools.
+   10. Right-click Security Settings in the top-left corner and choose Import Policy. This will allow you to choose a security template to import the settings from. Browse to your security template and choose it as the template to import.
+   11. Once the template is imported, verify the settings within the local security policy to make sure the template settings have been applied.
+   12. To refresh the policy, type **gpupdate /force** at a command prompt.
+   13. Log off and log back on as user **adminguy**. Do you get the new banner? __________
+   14. Why or why not? ________________________________

# Patch Management

As discussed earlier, applying patches to systems is a very important aspect of system hardening. As software vendors find out about vulnerabilities in their software, they ship out fixes for those flaws via patches. If you are not patching your systems, you are not receiving the fixes.

It is important that you plan a patching strategy instead of simply performing a Windows Update on every system. You can use software such as Windows Server Update Services (WSUS) to download, review, and deploy patches from a central server (see Figure 6-9).

![](bbox=[0.073, 0.594, 0.811, 0.946])