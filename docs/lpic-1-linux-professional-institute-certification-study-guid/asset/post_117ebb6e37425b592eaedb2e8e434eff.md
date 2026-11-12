
## Listing 10.50: Generating a key revocation document using the gpg utility (continued)

```
$ gpg --out key-revocation.asc --gen-revoke cb1234@ivytech.edu
[...]
Create a revocation certificate for this key? (y/N) y
Please select the reason for the revocation:
0 = No reason specified
1 = Key has been compromised
2 = Key is superseded
3 = Key is no longer used
Q = Cancel
(Probably you want to select 1 here)
Your decision? 1
[...]
$
$ ls key-revocation.asc
key-revocation.asc
$
```

After you have the revocation certificate, you can import it into your keyring to officially void your public/private key pair:

```
gpg --import FILENAME.asc
```

Next you need to make the revocation certificate available to others who may have your public key on their keyrings. The method you choose here depends on how you originally distributed the public key. If you used a GnuPG key server, then send the revocation certificate to it. If a website was used, publish the document to it. The holders of your public key then import the certificate using the same `gpg --import` command, which voids the key.

## Summary

In this chapter we took a look at how to set a basic level of host security. This included how to audit your network services and disable any unneeded ones. We also explored local security topics, such as securing account passwords, limiting root account access, and locating potentially dangerous programs.

To set some foundations, we delved into basic cryptography concepts. Those concepts were then expanded into practical knowledge on using OpenSSH to secure transmissions, and GnuPG to encrypt files.

These skills will assist you in securing your systems. As computer attacks grow, the systems in your care will be protected.

## Exam Essentials

### Describe the tools for auditing network services.
The nmap and netstat utilities will allow you to audit open ports and the services provided at these ports, but keep in mind the netstat command is deprecated. You can find network sockets using both the ss utility as well as via the systemctl command to view systemd managed sockets. Since Linux treats network connections and sockets as files, you may employ both the lsof and fuser utilities to audit these items as well.

### Summarize super servers and TCP Wrappers.
A super server directly listens for packets containing a designated port number, and when such a packet arrives, the server starts the matching network service. The extended super daemon, xinetd, uses the /etc/xinetd.conf configuration file for its global directives and specific service configuration files stored in the /etc/xinetd.d/ directory for its managed network services’ configurations. TCP Wrappers use the /etc/hosts.allow and /etc/hosts.deny files to determine who can access a particular system service. The hosts.allow file is checked first, and if a particular system is listed in that file, then access is allowed, and no further checks are made. However, if it is not found in that file, the hosts.deny file is checked. Since access is allowed if the remote system’s address is not found in either file, it is best to employ the ALL wildcard in the /etc/hosts.deny file, ALL: ALL.

### Clarify how Linux secures passwords.
Linux used to store account passwords as salted hashes in /etc/passwd, but because everyone can view the data in that file, they were moved to the more locked-down /etc/shadow. In order to change your password, you must employ the passwd utility, which is able to modify the /etc/shadow file contents due to the utility’s SUID settings.

### Explain how to limit root account access.
Though with the su utility you can quickly log into the other account or just issue a few commands, it does not provide a nonrepudiation environment. Therefore, it’s best to use the sudo command, which allows a user to issue a single command with super user privileges. The primary configuration file for this command is the /etc/sudoers file, though modifications are best made to configuration files stored in the /etc/sudoers.d/ directory for modern distributions.

### Outline setting limits on a Linux system.
To protect a system from having its resources consumed by a runaway program or threat actor, it is a good idea to limit resources, such as the number of logins, processes, or memory usage per user account. The ulimit utility provides these limits and is typically configured for user account in the environment files.

### Define locating SUID/SGID files.
Because SUID and SGID permissions set on a program or script can be a way for a malicious actor to cause problems on the system, it is wise to audit the files set with these permissions. The best utility to use is the find command. Search the entire virtual directory structure starting at the root directory (/), with the -perm /6000 switch and argument to find these files.

### Summarize using OpenSSH.
The OpenSSH utility allows you to log into a remote system through an encrypted network connection; its primary interface is the ssh command, and it employs this syntax: ssh [OPTIONS] USERNAME@HOSTNAME. It keeps track of any previously connected hosts in the ~/.ssh/known_hosts file, which contains the remote servers' public keys. Its configuration files include ~/.ssh/config (client configurations), /etc/ssh/ssh_config (global client configurations), and /etc/ssh/sshd_config (OpenSSH daemon configurations).

To manually generate the needed public/private key pair, use the ssh-keygen utility. To employ OpenSSH to copy the files to a remote system over an encrypted tunnel, use the scp command. To use password-less access, either copy over your public key to a remote system's ~/.ssh/authorized_keys file with the ssh-copy-id utility or authenticate with an authentication agent via ssh-agent and ssh-add commands. You can also tunnel X11 GUI interactions via the ssh -X user@remote-host command as long as tunneling is allowed.

### Describe GPG concepts.
The GPG utility allows you to encrypt and decrypt files using a public/private key pair. To generate the keys, you need to employ the gpg --gen-key command, which then stores the keys in a file (also called the keyring) within the ~/.gnupg/ directory. To share your public key, you must first export it from your keyring to a file via the --export option. The public key recipient then puts the key on their keyring via the --import switch. You can view the keys on your keyring by using the gpg --list-keys command. To encrypt a file, use the recipient's public key and the --encrypt option. When the encrypted file is received, the recipient decrypts the file with the private key and the --decrypt switch. There is no need to start a daemon to manage the private keys, because GPG handles its private keys via the gpg-agent daemon, which is started when needed by GPG. To protect against man-in-the-middle attacks, you can sign your encrypted files with the --sign or --clearsign option. The encrypted file recipient can then verify the signature with the --verify switch. To revoke a public key, create a revocation certificate via the --gen-revoke, import the certificate onto your keyring, and share the certificate with others who need to revoke your public key on their keyring as well.

## Review Questions

You can find the answers in the appendix.

- **1.** Which of the following utilities allows you to scan a system's network ports and see the services offered for each port, and you don't have to be logged onto the system you are scanning?
  - **A.** fuser
  - **B.** lsof
  - **C.** nmap
  - **D.** netstat
  - **E.** ss

- **2.** Nickie needs to scan his system to see what ports are listening for incoming TCP connections. He decides to use the netstat utility. What options should he employ? (Choose all that apply.)
  - **A.** -l
  - **B.** -u
  - **C.** -s
  - **D.** -T
  - **E.** -t

- **3.** Nickie knows that the netstat utility is deprecated, so he has decided to switch to the ss utility. He needs to scan his system to see what ports are listening for incoming TCP connections. What options should he employ? (Choose all that apply.)
  - **A.** -l
  - **B.** -u
  - **C.** -s
  - **D.** -T
  - **E.** -t

- **4.** Case is trying to lock down an older Linux system. He was surprised to find that it may be offering FTP services. He discovers in the /etc/services file that these older FTP services typically run on ports 20 and 21. What lsof command should he run to see if there are active connections to the FTP services?
  - **A.** lsof -i UDP
  - **B.** lsof -i 20:TCP
  - **C.** lsof -i ftp:TCP
  - **D.** lsof -i :ftp
  - **E.** lsof -i 20:ftp
