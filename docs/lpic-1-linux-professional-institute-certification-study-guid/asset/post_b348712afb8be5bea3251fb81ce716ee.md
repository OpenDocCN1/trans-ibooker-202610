
If you have not used ssh to log into a particular remote host in the past, you’ll get a scary-looking message like the one shown in Listing 10.31. The message just lets you know that this particular remote host is not in the known_hosts file. When you type yes at the message’s prompt, the host is added to the collective.

If you have previously connected to the remote server and you get a warning message that says WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED, pay attention. It’s possible that the remote server’s public key has changed. However, it may also indicate that the remote system is being spoofed or has been compromised by a malicious user.

The cp utility, which was covered back in Chapter 4, allows you to copy files on the same system. Similar to the cp utility, scp lets you copy files, but it employs SSH to copy the files to a remote system over an encrypted tunnel. To use the scp command, add the username@hostname before the destination file’s location. An example is shown in Listing 10.32.

## Listing 10.32: Using scp to securely transfer a file over SSH

```
$ ls Project42.txt
Project42.txt
$
$ scp Project42.txt Christine@192.168.0.104:~
Christine@192.168.0.104's password:
Project42.txt           100%    0     0.0KB/s   00:00
$
```

In Listing 10.32, the Project42.txt file is sent to a remote system using the scp command. Notice that the remote system’s username and IP address has an added colon (:). This is to designate that the file is being transferred to a remote system. If you did not add the colon, the scp command would not transfer the file. It would simply rename the file to a filename with Christine@ and tack on the IP address too.

After the colon, the file’s directory destination is designated. The ~ symbol indicates that you want to place the file in the user’s home directory. You could also give the file a new name, if desired.

Besides copying files, you can use OpenSSH to send commands to a remote system. Just add the command, between quotation marks, to the ssh command’s end. An example is shown in Listing 10.33.

## Listing 10.33: Using ssh to send a command to a remote system

```
$ ssh Christine@192.168.0.104 "ls Project42.txt"
Christine@192.168.0.104's password:
Project42.txt
$
```

In Listing 10.33, the command checks if our file was properly copied to the remote system. The Project42.txt file was successfully transferred.

![](img/ca7617340ddbe2360d36ce3428729fa9_609_0.png)

Over time users tend to ignore the WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED message. Thus, it's a good idea to help a user avoid experiencing this warning when first connecting to a system. To do that, you'll need to configure a known_hosts file for all the users on the system. However, you don't have to do it for each individual user. Instead, the ssh_known_hosts file is used by all users on the system and can contain the remote server's public keys for all the systems they will connect to. You will have to manually create the file, and it typically resides in the /etc/ssh/ directory. With the /etc/ssh/ssh_known_hosts file in place, users will receive the warning message only when something has changed and should be investigated. This will help in keeping your system more secure.

## Configuring SSH

It's a good idea to review the various OpenSSH configuration files and their directives. Ensuring that your encrypted connection is properly configured is critical for securing remote system communications. Table 10.3 lists the primary OpenSSH configuration files.

### TABLE 10.3 Primary OpenSSH configuration files

| Configuration File | Description |
| :--- | :--- |
| ~/.ssh/config | Contains OpenSSH client configurations. May be overridden by ssh command options. |
| /etc/ssh/ssh_config | Contains OpenSSH client configurations. May be overridden by ssh command options or settings in the ~/.ssh/config file. |
| /etc/ssh/sshd_config | Contains the OpenSSH daemon (sshd) configurations. |

If you need to make SSH configuration changes, it is essential to know which configuration file(s) to modify. The following guidelines can help:

- For an individual user's connections to a remote system, create and/or modify the client side's ~/.ssh/config file.
- For every user's connection to a remote system, create and modify the client side's /etc/ssh/ssh_config file.
- For incoming SSH connection requests, modify the /etc/ssh/sshd_config file on the server side.

Keep in mind that in order for an SSH client connection to be successful, besides proper authentication, the client and remote server's SSH configuration must be compatible.

There are several OpenSSH configuration directives. You can peruse them all via the man pages for the ssh_config and sshd_config files. However, there are a few vital directives for the sshd_config file:

- AllowTcpForwarding: Permits SSH port forwarding.
- ForwardX11: Permits X11 forwarding.
- PermitRootLogin: Permits the root user to log in through an SSH connection. (Default is yes.) Typically, should be set to no.
- Port: Sets the port number the OpenSSH daemon (sshd) listens on for incoming connection requests. (Default is 22.)

An example of why you might change the client's ssh_config or ~/.ssh/config file is when the remote system's SSH port is modified in the sshd_config file. In this case, if the client-side configuration files were not changed to match this new port, the remote user would have to modify their ssh command's options. An example of this is shown in Listing 10.34. In this listing, the remote Ubuntu server has OpenSSH listening on port 1138, instead of the default port 22, and the user must use the -p option with the ssh command to reach the remote server.

## Listing 10.34: Using ssh to connect to a nondefault port on a remote system

```
$ ssh -p 1138 192.168.0.104
[...]
Christine@192.168.0.104's password:
Welcome to Ubuntu 18.04.1 LTS (GNU/Linux 4.15.0-36-generic x86_64)
[...]
Christine@Ubuntu1804:~$
Christine@Ubuntu1804:~$ ip addr show | grep 192.168.0.104
     inet 192.168.0.104/24 [...]
Christine@Ubuntu1804:~$
Christine@Ubuntu1804:~$ exit
logout
Connection to 192.168.0.104 closed.
$
```

To relieve the OpenSSH client users of this trouble, create or modify the ~/.ssh/config file for individual users, or for all client users, modify the /etc/ssh/ssh_config file. Set Port to 1138 within the configuration file. This makes it easier on both the remote users and the system administrator.

## Generating SSH Keys

Typically, OpenSSH will search for its system's public/private key pairs. If they are not found, OpenSSH automatically generates them. These key pairs, also called *host keys*, are stored in the /etc/ssh/ directory within files. In Listing 10.35, key files' names are displayed.

## Listing 10.35: Looking at OpenSSH key files

```
$ ls -1 /etc/ssh/*key*
/etc/ssh/ssh_host_ecdsa_key
/etc/ssh/ssh_host_ecdsa_key.pub
/etc/ssh/ssh_host_ed25519_key
/etc/ssh/ssh_host_ed25519_key.pub
/etc/ssh/ssh_host_rsa_key
/etc/ssh/ssh_host_rsa_key.pub
$
```

In Listing 10.35, both private and public key files are shown. The public key files end in the .pub filename extension, whereas the private keys have no filename extension. The filenames follow this standard:

```
`ssh_host_KeyType_key`
```

The key filename's *KeyType* corresponds to the digital signature algorithm used in the key's creation. The different algorithm types you may see on your system are as follows:

- rsa (Rivest–Shamir–Adleman) is the oldest type, widely used, and highly supported.
- dsa (Digital Signature Algorithm) is a Federal Information Processing Standard for digital signatures. Deprecated.
- ecdsa (Elliptic Curve Digital Signature Algorithm) is an Elliptic Curve implementation of DSA.
- ed25519 is a variation of the Edwards-curve Digital Signature Algorithm (EdDSA) that offers better security than dsa and ecdsa.

> **NOTE**
>
> It is critical that the private key files are properly protected. Private key files should have a 0640 or 0600 (octal) permission setting and be root owned. However, public key files need to be world readable. File permissions were covered in Chapter 4.

There may be times you need to manually generate these keys or create new ones. To do so, use the `ssh-keygen` utility. In Listing 10.36, an example of using ssh-keygen is shown.
