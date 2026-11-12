Notice that after the vacuuming was completed, another journalctl --disk-usage command was issued, but it shows 320.0M instead of the 300M size set on the vacuum option. This is because though the disk-usage switch shows both active and archived journal files, the vacuum options work only on archived journal files.

> If you'd like to back up your active journal file(s), you should be fine simply copying them first. However, right before you make a copy, run the journalctl --sync command to ensure all the entries are moved from their queue into the file.

# Viewing Different Journal Files

If you need to retrieve a journal file from a rescued system but view it first or look at the entries in an archived or copied journal file, a few journalctl switches are available that can help.

Because journalctl looks for the active journal files in either the /run/log/journal or the /var/log/journal directory, you can point it to a different directory location where a copied or another system's journal file is located by using the -D directory-name or --directory=directory-name option.

If the file you are trying to view has a different name than system.journal or user-UID.journal, use the --file=pattern option on the journalctl command. Set the pattern to be the exact name of the file you wish to view. However, if there are several files, you can employ file globbing within the pattern (file globbing was covered in Chapter 4) to match several files.

If you have recently rescued your system and now have two or more journal files with entries to view, you can merge them. To do this, use the -m or --merge switches on the journalctl utility. Keep in mind this does not physically merge the journal files but instead merges their entries in the output for your perusal.

> The systemd-journald journal utility allows you to send your system's journals to a centralized journal host system via systemd-journal-remote. To view all the various journal files' entries on the central host, you'll need to employ the -m or --merge option when using the journalctl command.

# Making Journal Entries

Similar to using logger for syslog protocol programs, you can add journal entries from the command line or scripts using the systemd-cat tool. In order to do so, you must pipe your command's STDOUT into the utility:

```
command | systemd-cat
```

An example of employing the systemd-cat command is shown in Listing 7.39. Notice that the test message was successfully added to the active journal file.