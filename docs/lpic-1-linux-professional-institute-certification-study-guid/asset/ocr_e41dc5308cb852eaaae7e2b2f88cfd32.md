```
tcpdump:x:72:72::/sbin/nologin
avahi:x:70:70:Avahi mDNS/DNS-SD Stack:/var/run/avahi-daemon:/sbin/nologin
$
$ tail -n 2 /etc/passwd
tcpdump:x:72:72::/sbin/nologin
avahi:x:70:70:Avahi mDNS/DNS-SD Stack:/var/run/avahi-daemon:/sbin/nologin
$
$ tail -n +42 /etc/passwd
gnome-initial-setup:x:989:983::/run/gnome-initial-setup:/sbin/nologin
tcpdump:x:72:72::/sbin/nologin
avahi:x:70:70:Avahi mDNS/DNS-SD Stack:/var/run/avahi-daemon:/sbin/nologin
$
```

One of the most useful tail utility features is its ability to watch log files. Log files typically have new messages appended to the file’s bottom. Watching new messages as they are added is very handy. Use the -f (or --follow) switch on the tail command and provide the log filename to watch as the command’s argument. You will see a few recent log file entries immediately. As you keep watching, additional messages will display as they are being added to the log file.

> Some log files have been replaced on various Linux distributions, and now the messages are kept in a journal file managed by journald. To watch messages being added to the journal file, use the journalctl --follow command.

To end your monitoring session using tail, you must use the Ctrl+C key combination. An example of watching a log file using the tail utility is shown snipped in Listing 1.41.

# Listing 1.41: Watching a log file with the tail command

```
$ sudo tail -f /var/log/auth.log
[sudo] password for Christine:
Aug 27 10:15:14 Ubuntu1804 sshd[15662]: Accepted password [...]
Aug 27 10:15:14 Ubuntu1804 sshd[15662]: pam_unix(sshd:sess[...]
Aug 27 10:15:14 Ubuntu1804 systemd-logind[588]: New sessio[...]
Aug 27 10:15:50 Ubuntu1804 sudo: Christine : TTY=pts/1 ; P[...]
Aug 27 10:15:50 Ubuntu1804 sudo: pam_unix(sudo:session): s[...]
Aug 27 10:16:21 Ubuntu1804 login[10703]: pam_unix(login:se[...]
Aug 27 10:16:21 Ubuntu1804 systemd-logind[588]: Removed se[...]
^C
$
```