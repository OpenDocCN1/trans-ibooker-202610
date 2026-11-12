# # Employing Tools to Locate Files

Besides using the FHS as a guide, there are many ways to find files on your Linux system. In this section, we’ll explore several tools that assist in locating files.

# ## Using the which Command

The which command shows you the full path name of a shell command passed as an argument. Examples of using this utility are shown in Listing 4.53.

# Listing 4.53: Using the which command

```
$ which passwd
/usr/bin/passwd
$
$ which shutdown
/usr/sbin/shutdown
$
$ which line
/usr/bin/which: no line in (/usr/local/bin:/usr/bin:/usr/local/sbin:
/usr/sbin:/home/Christine/.local/bin:/home/Christine/bin)
$
$ echo $PATH
/usr/local/bin:/usr/bin:/usr/local/sbin:/usr/sbin:
/home/Christine/.local/bin:/home/Christine/bin
$
```

In the first example in Listing 4.53, the which command is used to find the program location of the passwd command. It displays the full path name of /usr/bin/passwd. The shutdown utility is located in an sbin directory. However, the line program is not installed on this system, and the which utility displays all the directories it searched to find the program. It uses the PATH environment variable, whose contents are also displayed in Listing 4.53, to determine which directories to search.

> Environment variables are configuration settings that modify your process’s environment. When you type a command (program) name, the PATH variable sets the directories Linux will search for the program binary. It is also used by other commands, such as the which utility. Note that directory names are separated by a colon (:) in the PATH list.

The which command is also handy for quickly determining if a command is using an alias. Listing 4.54 shows an example of this.