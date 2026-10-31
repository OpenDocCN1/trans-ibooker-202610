5. By combining multiple options, you can search just a specific host and only display the columns you want:

```
msf > services -c name,port,info -S Apache 192.168.216.10

Services
========

host            name    port    info
----            ----    ----    ----
192.168.216.10  ajp13   8009    Apache Jserv Protocol v1.3
192.168.216.10  http    8022    Apache Tomcat/Coyote JSP engine 1.1
192.168.216.10  ssl/http 8383  Apache httpd

msf >
```

In later chapters, we will address the remaining database commands, such as loot, creds, vulns, and notes.