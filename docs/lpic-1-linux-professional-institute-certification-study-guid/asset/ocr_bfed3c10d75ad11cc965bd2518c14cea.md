# Listing 4.54: Using the which command to see a command alias

```
$ which ls
alias ls='ls --color=auto'
    /usr/bin/ls

$

$ unalias ls

$

$ which ls
/usr/bin/ls

$
```

When the which utility is used on the ls command in Listing 4.54, it shows that currently the ls command has an alias. Thus, when you type ls, it is as if you have typed the ls --color=auto command. After employing the unalias command on ls, the which utility shows only the ls program's location.

# Using the whereis Command

Another command for locating files is the whereis utility. This utility allows you to locate any command's program binaries and locate source code files as well as any manual pages. Examples of using the whereis utility are shown in Listing 4.55.

# Listing 4.55: Employing the whereis command

```
$ whereis diff
diff: /usr/bin/diff /usr/share/man/man1/diff.1.gz
/usr/share/man/man1p/diff.1p.gz

$

$ whereis line
line:

$
```

The first command issued in Listing 4.55 searches for program binaries, source code files, and manual pages for the diff utility. In this case, the whereis command finds a binary file as well as two manual page files. However, when whereis is used to locate files for the line utility, nothing is found on the system.

# Using the locate Command

A very convenient and simple utility to use in finding files is the locate program. This utility searches a database, mlocate.db, which is located in the /var/lib/mlocate/ directory, to determine if a particular file exists on the local system. The basic syntax for the locate command is as follows:

```
locate [OPTION]... PATTERN...
```