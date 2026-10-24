For example, an unusual port mapping could be created that requires web clients to connect using port 4444 instead of the usual port 80. Then, that port could be mapped to port 80 on a private IP in the private network. This might sound like a clever obfuscation, and network administrators do this all the time. However, such a plan really doesn’t provide much security. A hacker can easily port scan the network and learn which ports are open.

Many applications can be configured to use arbitrary ports, allowing many different applications to reside behind a single public IP address. Many of these applications may provide a web interface, but others require specific client software for access. Either way, the client or browser software must be configured to access the public IP address and also provide the external port being mapped.

> **Security Through Obscurity**
> 
> Most port scans are vanilla and will look at every potential port—all 65,535 of them—but a hacker might be looking for specific open ports in a strobe scan. So, by mapping unusual ports to those services, you could potentially discourage those attackers, but then clients would need to append a nonstandard port to their requests. Security through obscurity will not provide a successful security plan.

The port might be in a separate field or appended to the IP address separated by a colon (:). So, for a web browser, this might be something like:

```
http://203.0.113.86:4444
```

which might map to 192.168.0.4:8888 on the private network, meaning that on the device using 192.168.0.4, the application being accessed is set up on port 8888.

> **A Word About Port Forwarding**
> 
> Previously, some standard port numbers were discussed, and every hacker knows these common ports well. Any time you can easily map nonstandard ports, rather than use common ports, you should. However, serious hackers will still find these open ports, so port forwarding alone can’t be thought of as a security technique.