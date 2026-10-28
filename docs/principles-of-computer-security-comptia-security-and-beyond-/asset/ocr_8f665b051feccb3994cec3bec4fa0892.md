There is a checkbox in Advanced Settings of Remote Desktop Settings that is checked by default; that would prevent Remote Desktop access from the Kali Linux VM. This command and registry change override that setting.

d. Run the command
`idletime`

to get an idea if there is a person actively using the exploited machine currently. This information is handy for deciding whether to continue with the next steps now or possibly after some time elapses.

e. In a new Z shell terminal, enter
`rdesktop -u hacker -p AAAbbb111 192.168.1.105`

but using the IP address of your Windows 10 VM instead.

If you completed Chapter 14, you’ll see the logon warning made with a GPO. If so, click the OK button.

You’ll see the username of hacker with the following message: “Another user is signed in. If you continue, they’ll be disconnected. Do you want to sign in anyway?” This is where the **idletime** command comes in handy. If you saw a value for **idletime** that indicates a user might not be in front of the machine (for example, if they haven’t touched the keyboard for around 10 minutes or so), now is a good time to try this. Click the Yes button.

On the Windows 10 VM, you’ll now see the following messages (with your machine’s name): “Do you want to allow WEISSMAN-CLIENT\hacker to connect to this machine?” Click OK to disconnect your session immediately or click Cancel to stay connected. No action will disconnect your session in 30 seconds. Assuming the user is not in front of their computer (based on the output from **idletime**), 30 seconds will elapse and the hacker account will be signed in. Now you’re not restricted by the Meterpreter shell or the Windows command prompt, and you can freely interact with the victim machine. Stealing the Windows password hashes and many more actions are now trivial to perform!

After you’ve completed your activities in the compromised machine,