# Listing 2.25: Displaying all installed packages with the dpkg -l command

```
$ dpkg -l
Desired=Unknown/Install/Remove/Purge/Hold
| Status=Not/Inst/Conf-files/Unpacked/half-conf/Half-inst/trig-aWait/Trig
|/ Err?=(none)/Reinst-required (Status,Err: uppercase=bad)
||/ Name Version Architecture Description
+++-==========-==========-============-========================================
ii accountsservic 0.6.45-1ubuntu amd64 query and manipulate accounts
ii acl 2.2.52-3build amd64 Access control list utilities
ii acpi-support 0.142 amd64 scripts for handling ACPI
ii acpid 1:2.0.28-1ub amd64 Advanced Config and Power
ii adduser 3.116ubuntu1 all add and remove users
[...]
iU zsh 5.4.2-3ubuntu amd64 shell with lots of features
$
```

Notice in Listing 2.25 that the installed packages have a status code before their name. The possible package status codes are shown in the first few lines as output by the dpkg command. For example, the last line that shows the zsh package displays the iU code. This means that while the package is installed (i), it is unpacked (U), but not configured, which is a problem. Earlier in Listing 2.23, we installed the packages, and the installation process denoted that a dependency, zsh-common, was missing.

Imagine not having to deal with missing package dependencies! Well, a new trend in package management may do just that. It revolves around building software packages to include not only the primary application, but all its dependencies as well. One new package management system that employs this new and exciting method is Snappy for the Ubuntu distribution. It uses the .snap file extension. The packages are called snap packages, and using Snappy requires installation of the snapd daemon.

For missing dependency problems, you can quickly check whether a particular package or library is installed via the dpkg -s action as shown snipped in Listing 2.26. Notice that as expected, the needed zsh-common package is not installed.

# Listing 2.26: Displaying an uninstalled package status with the dpkg -s command

```
$ sudo dpkg -s zsh-common
dpkg-query: package 'zsh-common' is not installed and
no information is available
[...]
$
```