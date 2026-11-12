If you have identified a nefarious user currently on your system, you can lock their account (covered in Chapter 7) and then kill their process ID (PID) in order to boot them off the system. If you'd like to keep any additional users from logging into the system, until you get the issue resolved create the nologin file. Do this by using super user privileges and issuing touch /etc/nologin at the command line. When this file exists, users will not be able to log into the system. If desired, you can add a message within the file to let users attempting to access the system know what's going on.

# # Displaying Access History with the last Utility

The last command pulls information from the /var/log/wtmp file and displays a list of accounts showing the last time they logged in or out of the system or if they are still logged on. A snipped example is shown in Listing 10.28.

```
**Listing 10.28: Using the last command**

$ last
Tim tty5 Thu Oct 4 09:57 still logged in
Kevin tty4 Thu Oct 4 09:57 still logged in
Rich tty3 Thu Oct 4 09:56 still logged in
Christin pts/1 192.168.0.102 Thu Oct 4 09:14 still logged in
Christin pts/0 192.168.0.102 Wed Oct 3 14:10 - 15:32 (01:22)
[...]
wtmp begins Thu Jul 26 16:30:32 2019
$
```

Be aware that the /var/log/wtmp file typically gets automatically rotated via the cron utility, which is covered in Chapter 9. If you need to gather information from old wtmp files, you can employ the -f switch. For example, you could type in last -f /var/log/wtmp.1 to view data from the /var/log/wtmp.1 file.

The last command and the various other utilities covered in these sections are rather helpful for auditing current users and discovering when they last logged into the system. In addition, they are helpful commands even when you are troubleshooting non-security-related problems.

# # Setting Login, Process, and Memory Limits

The ulimit command allows you to restrict access to system resources for each user account. This is very helpful to prevent a runaway program from accidentally causing an outage on your system.

Listing 10.29 shows the output from running the ulimit command with the -a option, which displays the settings for the current user account.