# How to do it...

+   1. There are several networking commands provided by Meterpreter, which we can display using the help command followed by net for the network. Let's have a quick look at each of them:
+   2. The arp command displays the host ARP cache:
+   3. The getproxy command allows us to see the current proxy configuration:

```
meterpreter > help net

Stdapi: Networking Commands
===========================

Command       Description
-------       -----------
arp           Display the host ARP cache
getproxy      Display the current proxy
              configuration
ifconfig      Display interfaces
ipconfig      Display interfaces
netstat       Display the network connections
portfwd       Forward a local port to a
              remote service
resolve       Resolve a set of host names
              on the target
route         View and modify the routing table
```

```
meterpreter > arp

ARP cache
=========

IP address    MAC address      Interface
----------    -----------      ---------
10.0.0.2      00:50:56:e7:ac:5c 19
10.0.0.129    00:0c:29:2d:94:ea 19
10.0.0.254    00:50:56:fb:75:cc 19
...
```

```
meterpreter > getproxy
Auto-detect : Yes
Auto config URL :
Proxy URL :
Proxy Bypass :
```