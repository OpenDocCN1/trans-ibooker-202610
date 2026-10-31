Here, 10.7.7.4 is the address of the Kali Linux machine, so that the victims will send the buffer to that server. Also, depending on the system's configuration, you may have to create the keys.txt file in the path specified in the code. In this example, it is the web root (/var/www/html/). Also, add write permissions or set the ownership to the Apache's user to prevent permission errors when the web server tries to update a local file:

```
touch /var/www/html/keys.txt
chown www-data /var/www/html/keys.txt
```

This is the simplest version of a key logger. A more sophisticated version could include the following:

+   - Timestamp of the capture
+   - Identifier of the user or machine sending the information
+   - Saving keys to a database to facilitate queries, grouping, and sorting
+   - Controlling functionality, such as starting and stopping key loggers, triggering actions on certain keys or combinations

![](bbox=[0.12, 0.55, 0.2, 0.65])

Capturing information from clients or users during a penetration test should be avoided when possible, although sometimes it's necessary for correct coverage of certain attack vectors. If this is the case, proper security measures must be taken on the transmission, storage, and handling of such information. If any information is sent to a server controlled by the penetration tester, communication must be encrypted using HTTPS, SSH, or other secure protocol. The storage must also be encrypted. Full disk encryption is recommended, but database and file encryption on top of it is also required. Furthermore, depending on the rules of engagement, secure erase of all information may be requested.