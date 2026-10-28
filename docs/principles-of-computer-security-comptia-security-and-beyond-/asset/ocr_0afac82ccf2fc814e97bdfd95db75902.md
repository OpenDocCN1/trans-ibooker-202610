policy. For Windows servers, **RemoteSigned** is the default execution policy.

+   - **RemoteSigned** All scripts and configuration files downloaded from the Internet must be signed by a trusted publisher.
+   - **Restricted** No configuration files will be loaded and no scripts will be run.
+   - **Undefined** There isn’t any execution policy set for the scope. An assigned execution policy will be removed from a scope as long as it wasn’t set by a Group Policy Object. If all scopes have an execution policy of **Undefined**, the effective execution policy is **Restricted**.
+   - **Unrestricted** With PowerShell 6.0, non-Windows machines have this as the non-changeable default execution policy. All configuration files will be loaded and all scripts will be run. A permission prompt will occur for an unsigned script downloaded from the Internet.

More on execution policies can be found at the following links:

https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.security/set-executionpolicy?view=powershell-7.1&viewFallbackFrom=powershell-7

https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_execution_policies?view=powershell-7

![](bbox=[0.12, 0.63, 0.18, 0.66])

# Step 2 To test this out, create a create a PowerShell script. PowerShell scripts have a .ps1 extension.

```
notepad script1.ps1
```

In the Notepad window, type **echo “Hello, World!”** and then save and exit by clicking the X in the upper-right corner of the window and then clicking the Save button.

![](bbox=[0.12, 0.84, 0.23, 0.87])

# Step 3 See protection mechanisms that are in place in both the GUI and CLI