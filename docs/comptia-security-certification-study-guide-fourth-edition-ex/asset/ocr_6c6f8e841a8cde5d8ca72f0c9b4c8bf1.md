# FIGURE 5-5

Removing programs that are set to autostart from the Run portion of the registry

![](bbox=[0.208, 0.128, 0.885, 0.317])

After killing the process, be sure to check the configuration areas on the system that normally start programs automatically. This would be the Startup program group in the Start menu and also the Run portion of the registry at HKLM\Software\Microsoft\Windows\CurrentVersion\Run (see Figure 5-5). In our example, if you run regedit.exe and navigate to the Run portion of the registry, you will see Patch.exe is set to automatically run when Windows reboots. You need to delete this entry.

# Other Malicious Software

In addition to viruses, there are other types of malicious software (known as malware) you need to be familiar with for the real world and for the Security+ certification exam. In this section, you learn about spyware, adware, and rootkits, to name a few of the common types of malicious software that threaten systems today.

# Spyware

Spyware is hidden software that monitors and collects information about you and your activities and then sends that information to a remote system for the hacker to review. For example, spyware could collect information on your surfing habits. Spyware has also been known to do more than just monitor for information; it has also been known to make changes to the system such as browser redirection (sending you to a different web page) and slowing down the network connection.

# Adware

Adware is software that automatically loads advertisements on the screen, typically in the form of a pop-up window. The advertisement is designed to entice you into purchasing a product or subscribing to a site.