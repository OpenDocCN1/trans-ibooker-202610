from Meterpreter, close the Remote Desktop window and run the following cleanup script in Meterpreter to remove the added account and cover your tracks:

```
run multi_console_command -r <file listed in output after you completed Step 6b>
```

Due to changes in the registry with Windows 10, the last operation will fail. Don’t worry about it.

In Chapter 11, you cracked Windows password hashes with Mimikatz. This step illustrates how you’d get into the system to get the hashes in the first place. Imagine the possibilities for an attacker, now, with full GUI access!

f. From Meterpreter, either reboot or shut down the Windows 10 VM (yes, you can perform those actions from Meterpreter) with **reboot** or **shutdown**, respectively.

That’s a wrap!

⏱ 10 MINUTES

# Lab Exercise 22.04: Armitage

Armitage is a GUI front end for the Metasploit Framework. You’re going to use it in this lab exercise to discover host systems. The detection of a high volume of traffic, which this lab exercise will generate, is also a great indication of an incident, which could lead to incident response.

# Learning Objectives

In this lab exercise, you’ll add another tool to your arsenal. At the end of this lab exercise, you’ll be able to

+   - Use Armitage to discover host systems
+   - Understand how this will appear as an incident on the target network
+   - Be able to take the information from this lab and use it for port scanning, the logical next step