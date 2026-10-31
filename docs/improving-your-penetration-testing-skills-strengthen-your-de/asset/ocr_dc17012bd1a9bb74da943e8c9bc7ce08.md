For lab purposes, we'll edit the OpenSSH server configuration `/etc/ssh/sshd_config` file to permit `root` login by changing the line `#PermitRootLogin without-password` to `PermitRootLogin yes` as you can see in the following example:

```
...
# Authentication:
#LoginGraceTime 2m
PermitRootLogin yes
#StrictModes yes
#MaxAuthTries 6
#MaxSessions 10
...
```

To start the OpenSSH service automatically on boot, run the `systemctl enable ssh` and finish the configuration by restarting the service using the `systemctl restart ssh` command, as follows:

```
root@kali:~# systemctl enable ssh
Synchronizing state of ssh.service with SysV service script with /lib/systemd/systemd-sysv-install.
Executing: /lib/systemd/systemd-sysv-install enable ssh
root@kali:~# systemctl restart ssh
root@kali:~#
```

This is fine for a lab environment but when performing penetration tests configure SSH to use cryptographic keys for logging in to the Kali Linux machine. This is much more secure than using only a password.

# Connecting to Kali using SSH

To connect to the Kali machine, all we need is an SSH client. Most Unix, Linux, and macOS operating systems already have an SSH client installed; however, if you are using Windows to connect to the Kali Linux machine, you will need to install a client such as PuTTY, which is one of the most popular and free SSH clients for Windows.