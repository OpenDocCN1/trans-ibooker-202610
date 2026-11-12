# Building the cron Table

All system users can have their own cron table (including the root user) for running scheduled jobs. Linux provides the crontab command for handling the cron table. To list an existing cron table, use the -l parameter:

```
$ crontab -l
no crontab for rich
$
```

By default, each user's cron table file doesn't exist. To add entries to your cron table, use the -e parameter. When you do that, the crontab command automatically starts the vi editor with the existing cron table, or an empty file if it doesn't yet exist.

# EXERCISE 9.1

# Writing a Bash Script to View the Password Information for System Users

This exercise walks through how to write a Bash script to view the password information for all user accounts configured on the Linux system.

+   1. Log into your Linux graphical desktop and open a command prompt window.

+   2. At the command prompt, open a text editor of your choice and create the text file pwinfo.sh by typing nano pwinfo.sh, pico pwinfo.sh, or vi pwinfo.sh.

+   3. Enter the following code into the new text file:

```
#!/bin/bash
# pwinfo.sh - display password information for all users
list=$(cut -d : -f 1 /etc/passwd)
for user in $list ; do
    echo Password information for $user
    sudo chage -l $user
    echo "--------"
done
```

+   4. Save the file using the appropriate save command for your editor.

+   5. Give yourself execute permissions to the file by typing chmod u+x pwinfo.sh.

+   6. Run the shell script by typing ./pwinfo.sh.

+   7. Enter your password at the sudo prompt.

You should see the chage password information listed for all of the user accounts configured on the system.