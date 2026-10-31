3. Search for a port number as follows:

```
msf > services -p 22

Services
========

host            port  proto  name  state  info
----            ----  -----  ----  -----  ----
192.168.216.10  22    tcp    ssh   open   OpenSSH 7.1 protocol 2.0
192.168.216.129 22    tcp    ssh   open   OpenSSH 4.7p1 Debian 8ubuntu1 protocol 2.0

msf >
```

4. Like the hosts command, we can use the -s option to search for specific strings:

```
msf > services -S Apache

Services
========

host            port  proto  name    state  info
----            ----  -----  ----    -----  ----
192.168.216.10  8009  tcp    ajp13   open   Apache Jserv Protocol v1.3
192.168.216.10  8022  tcp    http    open   Apache Tomcat/Coyote JSP engine 1.1
192.168.216.10  8383  tcp    ssl/http open  Apache httpd
192.168.216.129 80    tcp    http    open   Apache httpd 2.2.8 (Ubuntu) DAV/2
192.168.216.129 8009  tcp    ajp13   open   Apache Jserv Protocol v1.3
192.168.216.129 8180  tcp    http    open   Apache Tomcat/Coyote JSP engine 1.1

msf >
```