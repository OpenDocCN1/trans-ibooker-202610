+   - The Kali Linux VM you installed in Chapter 1

# Let’s Do This!

Start your hypervisor, VMware Workstation Player. In the menu on the left, select your Kali Linux VM. Then click Play Virtual Machine in the lower right. Enter the username/password pair you configured in Chapter 1. Open up a terminal and start executing commands in the same way you did in the previous two lab exercises.

1a–1c

Step 1 On certain Linux distributions the root account is locked by default because no password is configured for it. Having the root account locked by default greatly reduces the attack surface. When a user is logged in as root, all applications run with root privileges, and as a result, vulnerabilities or bugs in applications can cause damage to your system. If malware ran in the context of the root account, it could do significantly more damage than if it ran in the context of a regular user. Someone logged in as root could also do damage either accidentally or maliciously. Furthermore, if someone logged in as root leaves their machine unattended, another person could either accidentally or maliciously damage the system.

+   a. Give the root account a password and verify it (after entering your password at the sudo prompt), which unlocks it:

```
sudo passwd root
```

+   b. Relock the root account, but the password is preserved:

```
sudo passwd -l root
```

+   c. Unlock the root account:

```
sudo passwd -u root
```

If there is a password configured, you can log in to the root account now. Keep the root account as is, with a password and unlocked.

2a–2f

Step 2 There are multiple ways to su into root, which result in different