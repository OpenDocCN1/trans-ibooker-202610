If you have super user privileges, the chown command allows you to change both the owner and group assigned to a file or directory at the same time using the format

```
chown NEWOWNER:NEWGROUP FILENAMES
```

This is often preferred over using the separate chgrp command. You can also avoid the chgrp command altogether by using

```
chown :NEWGROUP FILENAMES
```

Using chown this way can be done without any super user privileges, but you do have to own the file and be a member of the specified new group.

# Controlling Access to Files

When ownership and group membership for a file or directory are set, Linux allows certain accesses based on those settings. This section walks through how Linux handles the basic permissions settings that you can assign to any file or directory on your system.

# Understanding Permissions

When you use the -l option, as shown in Listing 4.44, you’ll find lots of information concerning a file, including its permission settings.

# Listing 4.44: Viewing a file's long listing

```
$ ls -l cake.txt
-rw-rw-r--. 1 Christine Bakers 42 Apr 24 10:45 cake.txt
$
```

The data displayed in the long listing for a file or directory can be a little confusing. Here is a brief description of the different items along with their value in Listing 4.44:

+   - File type code (-)
+   - Permission string (rw-rw-r--)
+   - Hard link count (1)
+   - File owner (Christine)
+   - File group (Bakers)