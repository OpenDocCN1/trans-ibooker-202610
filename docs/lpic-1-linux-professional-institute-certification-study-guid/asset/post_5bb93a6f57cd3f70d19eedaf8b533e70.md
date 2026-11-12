
### TABLE 7.13 The common journalctl MATCHES parameter used for filtering (continued)

| Match | Description |
| --- | --- |
| `_TRANSPORT=transport` | Match only entries received by the specified `transport` method. |
| `_UDEV_SYSNAME=dev` | Match only entries received from the specified device. |
| `_UID=userid` | Match only entries made by the specified user ID. |

Keep in mind that Table 7.13 lists only a few of the commonly used `MATCHES`. To view all the various available `MATCHES`, type `man systemd-journal-fields` at the command line.

> To use journalctl to view all the various journal entries, you'll need to be logged in as the root user, use super user privileges, or typically belong to the systemd-journal group. Recognize that your distribution may use a different group for this task.

When you are looking for specific event entries in the journal, use the desired filters and options to target specific items. In snipped Listing 7.36, only today's entries for the ssh.service systemd service unit are displayed.

### Listing 7.36: Using filters with the journalctl command

```
$ sudo journalctl --since=today _SYSTEMD_UNIT=ssh.service
-- Logs begin at Wed 2018-07-25 12:02:39 EDT, end at Wed 2019-05-29 [...]
May 29 14:10:50 Ubuntu1804 sshd[772]: Server listening on 0.0.0.0 port 22.
May 29 14:10:50 Ubuntu1804 sshd[772]: Server listening on :: port 22.
May 29 14:10:55 Ubuntu1804 sshd[772]: Received SIGHUP; restarting.
[...]
May 29 16:09:38 Ubuntu1804 sshd[2047]: Connection closed by 127.0.0.1 [...]
May 29 16:22:58 Ubuntu1804 sshd[2112]: Accepted password for Christine [...]
May 29 16:22:58 Ubuntu1804 sshd[2112]: pam_unix(sshd:session): session [...]
$
```

Employing various journalctl filters makes digging through journal files much easier.

> The journalctl utility has a feature similar to using `tail -f` on a log file. Just employ the journalctl `-f` or `--follow` switch, and you will see the last few entries and additional entries as they are added to the journal. When you are done watching, press the Ctrl+C key combination to quit.

## Maintaining the Journal

Besides configuring a persistent journal and keeping the journal disk usage in check, you have a few manual management activities you can employ for maintaining your journal file(s).

You can check the current disk usage of the journal file(s) by employing the journalctl --disk-usage command. An example on an Ubuntu distro is shown in Listing 7.37.

### Listing 7.37: Checking journal file disk usage

```
$ journalctl --disk-usage
Archived and active journals take up 344.0M in the file system.
$
```

The output from the command in Listing 7.37 shows how much current disk space all the journal files, active and archived, are taking up on the partition.

While systemd-journald can automatically clean up disk space via settings in the journald.conf file, you can do so manually as well. In this case, you employ a vacuum... well, actually it's vacuum options available on the journalctl command:

- --vacuum-size
- --vacuum-time

As you would expect, --vacuum-size removes journal files until the disk space consumed by journal files reaches the designated size. You follow the option with a number and tack on a unit (K, M, G, or T) to set the size. Be aware that this removes only archived journal files and has no effect on any active journal files.

For the --vacuum-time option, you designate the oldest journal entries allowed, and the rest are deleted. The time is denoted with a number as well as a time unit (s, min, h, days, months, weeks, or years), such as 10months. Like the size option, the time option affects only archived journal files. The active files are left untouched.

While you can combine the two different switches if needed, the snipped example in Listing 7.38 uses only the size option to pare down on disk usage.

### Listing 7.38: Cleaning up journal file disk usage

```
$ journalctl --disk-usage
Archived and active journals take up 344.0M in the file system.
$
$ sudo journalctl --vacuum-size=300M
Deleted archived journal
[...]
Vacuuming done, freed 24.0M of archived journals from /var/log/journal/
e9af6ca5a8fb4a70b2ddec4b1894014d.
$
$ journalctl --disk-usage
Archived and active journals take up 320.0M in the file system.
$
```

Notice that after the vacuuming was completed, another journalctl --disk-usage command was issued, but it shows 320.0M instead of the 300M size set on the vacuum option. This is because though the disk-usage switch shows both active and archived journal files, the vacuum options work only on archived journal files.

> If you'd like to back up your active journal file(s), you should be fine simply copying them first. However, right before you make a copy, run the journalctl --sync command to ensure all the entries are moved from their queue into the file.

## Viewing Different Journal Files

If you need to retrieve a journal file from a rescued system but view it first or look at the entries in an archived or copied journal file, a few journalctl switches are available that can help.

Because journalctl looks for the active journal files in either the /run/log/journal or the /var/log/journal directory, you can point it to a different directory location where a copied or another system's journal file is located by using the -D directory-name or --directory=directory-name option.

If the file you are trying to view has a different name than system.journal or user-UID.journal, use the --file=pattern option on the journalctl command. Set the pattern to be the exact name of the file you wish to view. However, if there are several files, you can employ file globbing within the pattern (file globbing was covered in Chapter 4) to match several files.

If you have recently rescued your system and now have two or more journal files with entries to view, you can merge them. To do this, use the -m or --merge switches on the journalctl utility. Keep in mind this does not physically merge the journal files but instead merges their entries in the output for your perusal.

> The systemd-journald journal utility allows you to send your system's journals to a centralized journal host system via systemd-journal-remote. To view all the various journal files' entries on the central host, you'll need to employ the -m or --merge option when using the journalctl command.

## Making Journal Entries

Similar to using logger for syslog protocol programs, you can add journal entries from the command line or scripts using the systemd-cat tool. In order to do so, you must pipe your command's STDOUT into the utility:

```
command | systemd-cat
```

An example of employing the systemd-cat command is shown in Listing 7.39. Notice that the test message was successfully added to the active journal file.

### Listing 7.39: Adding a journal entry with the systemd-cat utility

```
$ echo "Test of systemd-cat" | systemd-cat
$
$ journalctl --no-pager | grep systemd-cat
May 30 17:43:46 Ubuntu1804 cat[2599]: Test of systemd-cat
$
```

If your system allows an installed syslog protocol program to act as a journal client by loading the `imuxsock` module, you can use the `logger` utility as well to make journal entries. An example of this is shown snipped in Listing 7.40.

### Listing 7.40: Adding a journal entry with the logger utility

```
$ logger "Test of logger"
$
$ journalctl -r
-- Logs begin at Thu 2018-07-26 18:19:45 EDT, end at Thu 2019-05-30 [...]
[...]
May 30 17:45:29 Ubuntu1804 Christine[2606]: Test of logger
[...]
May 30 17:43:46 Ubuntu1804 cat[2599]: Test of systemd-cat
[...]
$
```

By employing the `-r` option (reverse) on the `journalctl` command, you can see recent entries, which in this case includes both the `systemd-cat` and the `logger` utilities’ entries. Notice that `logger` adds the username to the entry.

Properly controlling your logs and journals is helpful in troubleshooting problems as well as tracking down potential security issues. Next, we’ll look at a concept that goes hand in hand with managing logs and journals.

## Maintaining the System Time

Keeping the correct times on all servers is crucial. Many elements depend on accurate time, such as programs designed to run at particular moments, remote services that expect accurate client times (and will reject the client if their times are inaccurate), and maintaining accurate log message time stamps in order to properly investigate client/server issues.

## Understanding Linux Time Concepts

Local time is also called _wall clock time_. Historically, people glanced at a clock on the wall (or their analog watches) to determine when to go to scheduled meetings, take lunch breaks, and leave work to go home. The official standard name for this is _localtime_.
