
You can see on this distribution that the home directory will be created by default,
because CREATE_HOME is set to yes. The SHELL directive is pointing to the Bash shell,
/bin/bash, which is the typical shell for most interactive user accounts.

The useradd command, as mentioned earlier, is the primary tool for creating user
accounts on most distributions. Creating an account on CentOS distribution with the
useradd utility is shown in Listing 7.7.

### Listing 7.7: Creating a user account on CentOS

```
$ sudo useradd DAdams
[sudo] password for Christine:
$
$ grep ^DAdams /etc/passwd
DAdams:x:1002:1002::/home/DAdams:/bin/bash
$
$ sudo grep ^DAdams /etc/shadow
DAdams:!!:17806:0:99999:7:::
$
$ sudo ls -a /home/DAdams/
. .. .bash_logout .bash_profile .bashrc .mozilla
$
```

Because the CentOS distribution we are using in Listing 7.7 has the CREATE_HOME directive set to yes and SHELL set to /bin/bash, there is no need to employ any useradd command options. The only argument needed is the user account name, which is DAdams. After the utility is used to create the account in Listing 7.7, notice that records now exist for the new user account in both the /etc/passwd and /etc/shadow files. Also, a new directory was created, /home/DAdams, which contains files from the /etc/skel/ directory. Keep in mind that at this point no password has been added to the DAdams account yet, and thus its record in the /etc/shadow file shows !! in the password field.

Now let’s take a look at creating an account on a different Linux distribution. The
Ubuntu Desktop distro does things a little differently. In Listing 7.8, you can see that
CREATE_HOME is not set, so it will default to no.

### Listing 7.8: Checking user account directives on Ubuntu Desktop

```
$ grep CREATE_HOME /etc/login.defs
$
$ useradd -D | grep SHELL
SHELL=/bin/sh
$
```

Also in Listing 7.8, notice that the SHELL directive is set to /bin/sh instead of the Bash
shell. This means that when you create an interactive user account, you will need to specify
the Bash shell, if desired.

Therefore, when creating a user account on this Ubuntu distribution, if you want the account to have a home directory and use the Bash shell, you will need to employ additional useradd command switches. The useradd utility has many useful options for various needs; the most typical ones are listed in Table 7.5.

### TABLE 7.5 The useradd command's commonly used options

| Short | Long | Description |
| :--- | :--- | :--- |
| -c | --comment | Comment field contents. Traditionally it contains the user's full name. Optional. |
| -d | --home or --home-dir | User's home directory specification. Default action is set by the HOME and CREATE_HOME directives. |
| -D | --defaults | Display /etc/default/useradd directives. |
| -e | --expiredate | Date of account's expiration in YYYY-MM-DD format. Default action is set by the EXPIRE directive. |
| -f | --inactive | Number of days after a password has expired and has not been changed until the account will be deactivated. A -1 indicates account will never be deactivated. Default action is set by the INACTIVE directive. |
| -g | --gid | Account's group membership, which is active when user logs into system (default group). |
| -G | --groups | Account's additional group memberships. |
| -m | --create-home | If it does not exist, create the user account's home directory. Default action is set by the CREATE_HOME directive. |
| -M | N/A or --no-create-home | Do not create the user account's home directory. Default action is set by the CREATE_HOME directive. |
| -s | --shell | Account's shell. Default action is set by the SHELL directive. |
| -u | --uid | Account's user identification (UID) number. |
| -r | --system | Create a system account instead of a user account. |

We need to employ a few of the switches in Table 7.5 to create a user account on the Ubuntu Desktop distribution. An example is shown in Listing 7.9.

### Listing 7.9: Creating a user account on Ubuntu Desktop

```
$ sudo useradd -md /home/JKirk -s /bin/bash JKirk
[sudo] password for Christine:
$
$ grep ^JKirk /etc/passwd
JKirk:x:1002:1002::/home/JKirk:/bin/bash
$
$ sudo grep ^JKirk /etc/shadow
JKirk:!:17806:0:99999:7:::
$
$ sudo ls -a /home/JKirk/
.  ..  .bash_logout  .bashrc  examples.desktop  .profile
$
$ sudo ls -a /etc/skel
.  ..  .bash_logout  .bashrc  examples.desktop  .profile
$
```

Notice in Listing 7.9 that three options are used along with the useradd command. Because this system does not have the CREATE_HOME directive set, the -m option is needed to force useradd to make a home directory for the account. The -d switch designates that the directory name should be /home/JKirk. Because the SHELL directive is set to /bin/sh on this system, the -s option is needed to set the account's default shell to /bin/bash.
After the utility is used to create the account in Listing 7.9, notice that records now exist for the new user account in the /etc/passwd and /etc/shadow files. Also, a new directory was created, /home/JKirk, which contains files from this distro's /etc/skel/ directory. Keep in mind at this point that no password has been added to the JKirk account yet, and thus its record in the /etc/shadow file shows ! in the password field.

![](img/ca7617340ddbe2360d36ce3428729fa9_416_0.png)

The Ubuntu and Debian distributions promote the use of the adduser program instead of the useradd utility. Their man pages refer to the useradd command as a “low-level utility.” Some other distros include a symbolic link to useradd named adduser, which may help (or not). The adduser configuration information is typically stored in the /etc/adduser.conf file.

Another way to view account records in the /etc/passwd and /etc/shadow files is via the getent utility. For this program you pass only the filename followed by the account name whose record you wish to view. The command is employed in Listing 7.10 to view the account that was created in Listing 7.9.

### Listing 7.10: Using getent to view a user account on Ubuntu Desktop

```
$ getent passwd JKirk
JKirk:x:1002:1002::/home/JKirk:/bin/bash
$
$ getent shadow JKirk
$
$ sudo getent shadow JKirk
JKirk:!:17806:0:99999:7:::
$
```

Notice in Listing 7.10 that when super user privileges are not used with getent for the shadow file, nothing is returned. This is because getent honors the security settings on the /etc/shadow file.

If you need to modify the /etc/default/useradd file's directive settings, instead of using a text editor you can employ the useradd -D command. Just tack on the needed arguments. For example, to modify the SHELL directive to point to the Bash shell, use super user privileges and issue the useradd -D -s /bin/bash command.

When creating an account, you can create a password via the crypt utility and then add it when the account is created via the -p option on the useradd utility. However, not only is that cumbersome, but it's also considered a bad practice. In the next section, we'll cover creating and managing account passwords properly.

## Maintaining Passwords

When you first create an interactive account, you should immediately afterward create a password for that account using the passwd utility. In Listing 7.11, a password is created for the new DAdams account on a CentOS system.

### Listing 7.11: Using passwd for a new account on CentOS

```
$ sudo passwd DAdams
Changing password for user DAdams.
New password:
Retype new password:
passwd: all authentication tokens updated successfully.
$
```

You can also update a password for a particular user using the passwd utility and pass the user's account name as an argument, similar to what is shown in Listing 7.11. If you need to update your own account's password, just enter passwd with no additional command arguments.

You can do more than set and modify passwords with the passwd utility. You can also lock or unlock accounts, set an account's password to expired, delete an account's password, and so on. Table 7.6 shows commonly used passwd switches; all of these options require super user privileges.

### TABLE 7.6 The passwd command's commonly used options

| Short | Long | Description |
| :--- | :--- | :--- |
| -d | --delete | Removes the account's password. |
| -e | --expire | Sets an account's password as expired. User is required to change account password at next login. |
| -i | --inactive | Sets the number of days after a password has expired and has not been changed until the account will be deactivated. |
| -l | --lock | Places an exclamation point (!) in front of the account's password within the /etc/shadow file, effectively preventing the user from logging into the system using the account's password. |
| -n | --minimum | Sets the number of days after a password is changed until the password may be changed again. |
| -S | --status | Displays the account's password status. |
| -u | --unlock | Removes a placed exclamation point (!) from the account's password within the /etc/shadow file. |
| -w | --warning or --warndays | Sets the number of days a warning is issued to the user prior to a password's expiration. |
| -x | --maximum or --maxdays | Sets the number of days until a password change is required. This is the password's expiration date. |

One option in Table 7.6 needs a little more explanation, and that is the -S option. An example is shown in Listing 7.12.

### Listing 7.12: Using passwd -S to view an account's password status

```
$ sudo passwd -S DAdams
DAdams PS 2018-10-01 0 99999 7 -1 (Password set, SHA512 crypt.)
$
```
