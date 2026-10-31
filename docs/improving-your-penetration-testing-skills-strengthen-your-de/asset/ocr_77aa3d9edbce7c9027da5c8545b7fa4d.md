6. Editing files using Meterpreter can be done by using the edit command, which uses vim so all the editor's commands are available:

```
meterpreter > edit flag.txt
```

7. One of my favorite commands is the show_mount command, which allows you to list all mount points/logical drives in the target system:

```
meterpreter > show_mount
```

```
Mounts / Drives
================

Name    Type    Size (Total)    Size (Free)    Mapped to
----    ----    ------------    -----------    ---------
C:\     fixed   60.00 GiB       42.31 GiB

Total mounts/drives: 1
```

8. To display all the available commands, you can use the help command followed by the group of commands you want to display:

```
meterpreter > help File system Commands
```

```
Stdapi: File system Commands
===========================

Command      Description
-------      -----------
cat          Read the contents of a file
             to the screen
cd           Change directory
checksum     Retrieve the checksum of a
             file.
...
```

# How it works...

Meterpreter gives us complete access to the target machine by setting up an interactive command prompt. We can also drop a shell session to work in the default Windows DOS mode, but it will not have as many functionalities. This was a quick reference to some of the important filesystem commands of Meterpreter, which can help us in exploring the files present on the target machine. There are more commands as well; it is recommended that you try them out and find the various possibilities which exist.