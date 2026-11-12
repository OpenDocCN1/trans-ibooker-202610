There are a few methods you can use while auditing systemd managed network sockets, and the best place to start is via the systemctl utility (first covered in Chapter 5, “Booting, Initializing, and Virtualizing Linux”). In snipped Listing 10.6, the command is used along with the list-sockets argument and several options to make the display a little easier to read.

# Listing 10.6: Viewing systemd managed sockets using the systemctl utility

```
$ systemctl list-sockets --all --no-pager --full
LISTEN UNIT ACTIVATES
[...]
/var/run/cups/cups.sock cups.socket cups.service
[...]
0.0.0.0:111 rpcbind.socket rpcbind.service
0.0.0.0:111 rpcbind.socket rpcbind.service
[...]
[:::]:111 rpcbind.socket rpcbind.service
[:::]:111 rpcbind.socket rpcbind.service
[:::]:22 sshd.socket
[...]
28 sockets listed.
$
```

You’ve probably already noticed that you will see many more types of sockets besides just network sockets in this command’s output. The trick is to look for familiar network service names (example: sshd) and/or port numbers (example: 22) in the output.

To find the potential network socket systemd configuration (unit) files, employ a different systemctl command, as shown snipped in Listing 10.7.

# Listing 10.7: Viewing systemd managed socket unit files using the systemctl utility

```
$ systemctl list-unit-files --type=socket --no-pager
UNIT FILE STATE
[...]
cups.socket enabled
[...]
rpcbind.socket enabled
[...]
sshd.socket disabled
[...]

32 unit files listed.
$
```