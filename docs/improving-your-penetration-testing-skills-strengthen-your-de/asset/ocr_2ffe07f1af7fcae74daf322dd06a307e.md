A malicious user might use the following request to pipe in additional commands, which the application would accept without raising an exception:

```
http://server/page.php?ip=127.0.0.1;uname -a
```

The application takes the value of the user input from the client without validation and concatenates it to the ping -c 3 command in order to build the final command that is run on the web server. The response from the server is shown in the following screenshot. The version of the underlying OS is displayed along with the result of pinging the given address as the application failed to validate the user input:

![](bbox=[0.125, 0.295, 0.87, 0.545])

The additional command injected will run using the privileges of the web server. Most web servers nowadays run with restricted privileges, but even with limited rights, the attacker can exploit and steal significant information.

Command injection can be used to make the server download and execute malicious files by injecting the wget commands, or to gain a remote shell to the server, as demonstrated in the following example.

First, set up a listener in Kali Linux. Netcat has a very simple way of doing this:

```
nc -lvp 12345
```