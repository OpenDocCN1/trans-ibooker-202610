and intrusion prevention systems (IPSs), as well as remove bad characters that would crash a target program or system.

+   - **MSFvenom** Combines payload generation and encoding.
+   - **MSFconsole** The most popular interface to the Metasploit Framework. MSFconsole is an all-in-one centralized console to work from.
+   - **PostgreSQL (pronounced *post-gres-Q-L*)** An open-source relational database management system (RDBMS) used by Metasploit.
+   - **Meterpreter** Metasploit’s most popular payload. Meterpreter allows you to do many things on the victim machine, including uploading, downloading, creating, modifying, and deleting files, taking screenshots, watching the victim machine live, taking over the screen, mouse, and keyboard, turning on a webcam, and much more.

In the following steps, be sure to press ENTER after each command.

# 1a–1d

# Step 1 Open a terminal in your Kali Linux VM. Create an executable file with a Meterpreter payload, start a web server, and copy the executable file to the root directory of the web server.

+   a. First, use MSFvenom to generate a 64-bit Windows executable file that implements a reverse TCP shell, Meterpreter, for the payload.

You’ll recall from Chapter 16 that a bind shell is created from the attacker’s machine directly to the victim’s machine, and it allows the attacker to execute commands on the victim’s machine. Firewalls and Network Address Translation (NAT) can get in the way of bind shells. Therefore, pentesters and attackers might decide to use a reverse shell, which goes in the other direction. It’s a connection initiated from the inside victim’s machine directly to the outside attacker’s machine. This also comes in handy when an attacker strategically “drops” a USB drive in a company’s parking lot and waits for an employee to plug it in. The attacker doesn’t know how the networking is set up, but that victim machine will run something on the USB that creates a reverse shell to the attacker’s machine,