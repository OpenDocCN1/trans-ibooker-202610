
In Listing 7.12, the DAdams account's password status is displayed. The status contains the account password's state, which is either a usable password (P), no password (NP), or a locked password (L). After the password state, the last password change date is shown, followed by the password's minimum, maximum, warning, and inactive settings. Additional status is shown within the parentheses, which includes whether or not the password is set as well as the hash algorithm used to protect it.

You can also use the chage utility to display similar password information but in a more human-readable format, as shown in Listing 7.13.

### Listing 7.13: Using chage -l to view an account's password status

```
$ sudo chage -l DAdams
Last password change                                    : Oct 02, 2018
Password expires                                        : never
Password inactive                                       : never
Account expires                                         : never
Minimum number of days between password change          : 0
Maximum number of days between password change          : 99999
Number of days of warning before password expires       : 7
$
```

The chage program can modify password settings as well. You can either employ various command options (see its man pages for details) or use the chage utility interactively, as shown in Listing 7.14.

### Listing 7.14: Using chage to change an account password's settings

```
$ sudo chage DAdams
Changing the aging information for DAdams
Enter the new value, or press ENTER for the default

            Minimum Password Age [0]: 5
            Maximum Password Age [99999]: 30
            Last Password Change (YYYY-MM-DD) [2018-10-02]:
            Password Expiration Warning [7]: 15
            Password Inactive [-1]: 3
            Account Expiration Date (YYYY-MM-DD) [-1]:

$
```

Notice in Listing 7.14 that the password expiration warning is set to 15 days. This is a good setting if your company allows two-week vacations.

## Modifying Accounts

The utility employed to modify accounts is the usermod program. Similar to the passwd command, you can lock and unlock accounts, as shown in Listing 7.15.

### Listing 7.15: Using usermod to lock an account

```
$ sudo usermod -L DAdams
$
$ sudo passwd -S DAdams
DAdams LK 2018-10-01 5 30 15 3 (Password locked.)
$
$ sudo getent shadow DAdams
DAdams:!$6$6B/zCaNx[...]:17806:5:30:15:3:::
$
$ sudo usermod -U DAdams
$
$ sudo passwd -S DAdams
DAdams PS 2018-10-01 5 30 15 3 (Password set, SHA512 crypt.)
$
```

Notice in Listing 7.15 that the usermod -L command is used to lock the DAdams account. The passwd -S command shows the password status is LK, indicating it is locked. In Listing 7.15, the snipped getent utility output shows that an exclamation point (!) was placed in front of the DAdams account's password, which is what is causing the account to be locked. The lock is then removed via the usermod -U command and the status is rechecked. You can make many modifications to user accounts via the usermod utility's switches. The commonly used switches are shown in Table 7.7.

### TABLE 7.7 The usermod command's commonly used options

| Short | Long | Description |
|---|---|---|
| -c | --comment | Modify the comment field contents. |
| -d | --home | Set a new user home directory specification. Use with the -m option to move the current directory's files to the new location. |
| -e | --expiredate | Modify the account's expiration date. Use YYYY-MM-DD format. |
| -f | --inactive | Modify the number of days after a password has expired and has not been changed that the account will be deactivated. A -1 indicates account will never be deactivated. |
| -g | --gid | Change the account's default group membership. |
| -G | --groups | Update the account's additional group memberships. If only specifying new group membership, use the -a option to avoid removing the other group memberships. |
| -l | --login | Modify the account's username to the specified one. Does not modify the home directory. |
| -L | --lock | Lock the account by placing an exclamation point in front of the password within the account's /etc/shadow file record. |
| -s | --shell | Change the account's shell. |
| -u | --uid | Modify the account's user identification (UID) number. |
| -U | --unlock | Unlock the account by removing the exclamation point from the front of the password within the account's /etc/shadow file record. |

Notice that you can change an account's default group and provide memberships to additional groups. Account groups are covered in detail later in this chapter.
Where usermod comes in handy is in a situation where you've created an account but forgot to check the distribution's account creation configuration settings. Listing 7.16 shows an example of this on an Ubuntu Desktop distribution.

### Listing 7.16: Using usermod to modify an account

```
$ sudo useradd -md /home/DBowman DBowman
$
$ sudo getent passwd DBowman
DBowman:x:1003:1003::/home/DBowman:/bin/sh
$
$ sudo usermod -s /bin/bash DBowman
$
$ sudo getent passwd DBowman
DBowman:x:1003:1003::/home/DBowman:/bin/bash
$
```

In Listing 7.16, the user account DBowman is created, but when the account record is checked using the getent utility, it shows that the /bin/sh shell is being used instead of the Bash shell. To fix this problem, the usermod command is employed with the -s option, and the account's shell is modified to the /bin/bash shell instead.

## Deleting Accounts

Deleting an account on Linux is fairly simple. The userdel utility is the key tool in this task. The most common option to use is the -r switch. This option will delete the account's home directory tree and any files within it. Listing 7.17 shows an example of deleting an account.

### Listing 7.17: Using userdel to delete an account

```
$ sudo ls -a /home/DBowman
.  .. .bash_logout .bashrc examples.desktop .profile
$
$ sudo getent passwd DBowman
DBowman:x:1003:1003::/home/DBowman:/bin/bash
$
$ sudo userdel -r DBowman
userdel: DBowman mail spool (/var/mail/DBowman) not found
$
$ sudo ls -a /home/DBowman
ls: cannot access '/home/DBowman': No such file or directory
$
$ sudo getent passwd DBowman
$
```

The first two commands in Listing 7.17 show that the /home/DBowman directory exists and has files within it and that the account does have a record within the /etc/passwd file. The third command includes the userdel -r command to delete the account as well as the home directory. Notice that an error message is generated stating that the /var/mail/DBowman file could not be found. This is not a problem. It just means that this file was not created when the account was created. Finally, the last two commands show that both the /home/DBowman directory and its files were removed and that the /etc/passwd file no longer contains a record for the DBowman account.

### Real World Scenario

### Account Deletion Policies

Prior to deleting any accounts on a system, check with your employer's human resources staff and/or legal department or counsel. There may be policies in place concerning file retention for terminated or retired employees as well as those individuals who have left the company to change jobs. You may be required to back up files prior to deleting them from the system and/or perform some other types of documentation. If your employer has no such policy, it is a good idea to suggest that one be developed.

## Configuring Groups

Groups are identified by their name as well as their group ID (GID). This is similar to how users are identified by UIDs in that the GID is used by Linux to identify a particular group, whereas humans use group names.

If a default group is not designated when a user account is created, then a new group is created. This new group has the same name as the user account's name and it is assigned a new GID. To see an account's default group, you can use the getent command to view the /etc/passwd record for that account. Recall that the fourth field in the record is the account's GID, which is the default group. Review Table 7.3 earlier in the chapter if you need a refresher on the various /etc/passwd record fields. Listing 7.18 shows an example of viewing an account's default group information for the DAdams account, which was created on a CentOS distribution.

### Listing 7.18: Viewing an account's group memberships

```
$ getent passwd DAdams
DAdams:x:1002:1002::/home/DAdams:/bin/bash
$
$ sudo groups DAdams
DAdams : DAdams
$
$ getent group DAdams
DAdams:x:1002:
$
$ grep 1002 /etc/group
DAdams:x:1002:
$
```

The first command in Listing 7.18 shows that the DAdams account's default group has a GID of 1002, but it does not provide a group name. The groups command does show the group name, which is the same as the user account name, DAdams. This is typical when no default group was designated at account creation time. The third command, another getent command, shows that the group DAdams does indeed map to the 1002 GID. The fourth command confirms this information.

To add a user to a new group or change the account's default group, the group must pre-exist. This task is accomplished via the groupadd utility. The group's GID will be automatically set by the system, but you can override this default behavior with the -g command option. An example of creating a new group is shown in Listing 7.19.

### Listing 7.19: Using the groupadd utility to create a new group

```
$ sudo groupadd -g 1042 Project42
$
$ getent group Project42
Project42:x:1042:
$
$ grep Project42 /etc/group
Project42:x:1042:
$
```
