```
Identity added: /home/Christine/.ssh/id_ecdsa (/home/Christine/.ssh/id_ecdsa)
[Christine@localhost ~]$
[Christine@localhost ~]$ ssh Christine@192.168.0.104
Welcome to Ubuntu 18.04.1 LTS (GNU/Linux 4.15.0-36-generic x86_64)
[...]
Christine@Ubuntu1804:~$ exit
logout
Connection to 192.168.0.104 closed.
[Christine@localhost ~]$
[Christine@localhost ~]$ exit
exit
$
```

Notice in Listing 10.41 that the ssh-agent command is followed by /bin/bash, which is the Bash shell. This command starts a new session, an agent session, with the Bash shell running. After the private SSH ID key is added using the ssh-add command and entering the private passphrase, you can connect to remote systems without entering a password or passphrase again. However, if you exit the agent session and start it up again, you must readd the key and reenter the passphrase.

The ssh-add command allows you to remove ID within an agent session, if you want. Include the -d option to do so.

An SSH agent session allows you to enter the session one time and add the key, and then connect as often as needed to remote systems via encrypted SSH methods without entering a password or passphrase over and over again. Not only does this provide security, it also provides convenience, which is a rare combination.

# Tunneling

Another way to provide security through OpenSSH is via SSH port forwarding, sometimes called SSH tunneling. SSH port forwarding allows you to redirect a connection from one particular network port to port 22, where the SSH service is waiting to receive it. This allows data traffic to move back and forth through a secure encrypted tunnel, similar to a virtual private network.

If you need to provide remote X11 GUI interactions, you can employ OpenSSH to use a secure tunnel. This is called X11 forwarding. X11 forwarding lets you interact with various X11-based graphical utilities on a remote system through an encrypted network connection (X11 forwarding is covered in Chapter 6).

First check to see if X11 forwarding is permitted in the openSSH configuration file, /etc/ssh/sshd_config. The directive X11Forwarding should be set to yes in the remote system’s configuration file. If the directive is set to no, then you must modify it to employ X11 forwarding. In Listing 10.42, a check is performed on the configuration file for this directive on a CentOS distribution.