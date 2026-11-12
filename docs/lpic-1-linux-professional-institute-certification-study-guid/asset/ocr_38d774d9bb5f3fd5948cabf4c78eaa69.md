One handy fact about this systemctl command’s output is that you can quickly determine whether the network socket is enabled or disabled for systemd. A disabled value only means systemd does not manage that particular network socket; it does *not* mean the service is disabled. In Listing 10.7, the rpcbind socket is enabled, whereas the sshd socket shows as disabled.

For the enabled sockets, you need to do a little more research before you can call this part of the audit complete. Take a look at the contents of each enabled network socket’s unit file. An example of viewing the rpcbind socket unit file is shown in Listing 10.8.

# Listing 10.8: Viewing contents of a systemd socket unit file using the systemctl utility

```
$ systemctl cat rpcbind.socket
# /usr/lib/systemd/system/rpcbind.socket
[Unit]
Description=RPCbind Server Activation Socket

[Socket]
ListenStream=/var/run/rpcbind.sock

# RPC netconfig can't handle ipv6/ipv4 dual sockets
BindIPv6Only=ipv6-only
ListenStream=0.0.0.0:111
ListenDatagram=0.0.0.0:111
ListenStream=[:::]:111
ListenDatagram=[:::]:111

[Install]
WantedBy=sockets.target
$
```

The unit file’s contents will give you more clues as to what port this socket is using (111 in this case), as well as additional network socket configuration information. If desired, you can view the various potential settings of these socket unit files by typing **man systemd.socket** at the command line.

# Auditing Open Files with lsof and fuser

Besides viewing ports and network sockets, you can use open files to trace down offered network services on your system. The *lsof* command lists currently open files. Since Linux treats network connections and sockets as files, they will appear in the *lsof* output list.

The *lsof* command produces lots of output, so it’s best to filter it. For example, if you’re interested in auditing only UDP protocol sockets and connections, use the `-iUDP` option as shown snipped in Listing 10.9. Notice you can see both IPv4 and IPv6 along with additional helpful auditing information.