# Changing Special Access Modes

There are three special permission bits that Linux uses for controlling advanced behavior of files and directories: SUID, SGID, and the sticky bit.

# Looking at SUID

The Set User ID (SUID) bit is used with executable files. It tells the Linux kernel to run the program with the permissions of the file owner and not the user account actually running the file. This feature is most commonly used in server applications that must run as the root user account to have access to all files on the system, but the Linux system starts them as a standard user account.

The SUID bit is indicated by an s in place of the execute permission letter for the file owner: rwsr-xr-x. The execute permission is assumed for the system to run the file. If the SUID bit is set on a file that doesn't have execute permission for the owner, it's indicated by a capital S.

A practical example of SUID on Linux is the passwd utility. The passwd utility allows you to change your password, which is stored in the /etc/shadow file (covered in Chapter 7). Because the shadow file only allows the root user (the file's owner) to write to it, you must temporarily gain the root user's permission status. This is done via the SUID permission set on the passwd program's file as shown in Listing 4.51.

# Listing 4.51: Viewing the passwd utility’s and /etc/shadow file’s permissions

```
$ ls -l /etc/shadow
-rw-r----- 1 root shadow 1425 Mar 21 17:51 /etc/shadow
$
$ which passwd
/usr/bin/passwd
$
$ ls -l /usr/bin/passwd
-rwsr-xr-x 1 root root 59640 Jan 25 2018 /usr/bin/passwd
$
```

To set the SUID bit for a file, in symbolic mode add s to the owner permissions, or in octal mode include a 4 at the start of the octal mode setting.

```
# chmod u+s myapp
# chmod 4750 myapp
```

# Looking at SGID

The Set Group ID (SGID) bit works differently in files and directories. For files, it tells Linux to run the program file with the file's group permissions. It's indicated by an s in the group execute position: rwxrwsr--. Like SUID, if the execute permission is not granted, the setting is benign and shown as a capital S in the group execute position.