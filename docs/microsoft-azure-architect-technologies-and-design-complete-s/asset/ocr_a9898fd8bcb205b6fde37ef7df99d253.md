# FIGURE 5.11 A portion of the storage account overview blade

![](bbox=[0.26, 0.135, 0.75, 0.305])

4. Open Azure PowerShell and enter the following PowerShell cmdlet:

```
Test-NetConnection -ComputerName <name>.file.core.windows.net -Port 445
```

If port 445 is blocked by your company or private firewall, then the previous PowerShell cmdlet will not work. Replace <name> with the name of the storage account you created in step 2. The output of the PowerShell cmdlet may resemble something similar to Figure 5.12.

# FIGURE 5.12 The output of a PowerShell cmdlet testing file share connectivity

![](bbox=[0.23, 0.505, 0.795, 0.595])

5. Click the file share you just created, click the Connect button, review the sample scripts provided per operating system, and select your desired OS script.

6. If the PowerShell cmdlet you ran in step 4 was successful, enter the following to mount/map the drive:

```
cmd.exe /C "cmdkey /add:'<name>.file.core.windows.net' /user:'Azure\<name>' /pass:'<password>'"
New-PSDrive -Name Z -PSProvider FileSystem -Root "\\<name>.file.core.windows.net\<shareName>" -Persist
```