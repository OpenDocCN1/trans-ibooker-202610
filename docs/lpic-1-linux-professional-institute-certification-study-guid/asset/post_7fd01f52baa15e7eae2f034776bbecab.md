
## Listing 1.4: Displaying the current shell on an Ubuntu distribution

```
$ uname
Linux
$
$ uname -r
4.15.0-46-generic
$
$ uname -a
Linux Ubuntu1804 4.15.0-46-generic #49-Ubuntu SMP Wed Feb 6
09:33:07 UTC 2019 x86_64 x86_64 x86_64 GNU/Linux
$
```

When used by itself, the `uname` command displays only the kernel’s name (Linux). If you want to know the current kernel version (called the _revision_), add the `-r` command option, as shown in Listing 1.4. To see all system information this utility provides, such as the processor type (`x86_64`) and operating system name (`GNU/Linux`), tack on the `-a` option to the `uname` command.

## Using a Shell

To run a program from the shell, at the command line you simply type its command, using the proper syntax, and press Enter to execute it. The `echo` command, used earlier, is a great program to start with. Its basic syntax is as follows:

```
echo [OPTION]... [STRING]...
```

In the `echo` command’s syntax structure, `[OPTION]` means there are various options (also called _switches_) you can add to modify the display. The brackets indicate that switches are optional. The `[STRING]` argument allows you to designate a string to display. It too is optional, as denoted by the brackets.

An example is helpful to understand the `echo` command’s syntax. In Listing 1.5 the `echo` command is employed with no arguments or options and simply displays a blank line. In its second use, the command shows the string provided as an argument to the `echo` program.

## Listing 1.5: Using the echo command

```
$ echo

$ echo I Love Linux
I Love Linux
$
```

## Quoting Metacharacters

The `echo` command is handy to demonstrate another useful shell feature: shell quoting. Within the Bash shell are several characters that have special meanings and functions. These characters are called *metacharacters*. Bash shell metacharacters include the following:

- ? [ ] ' " \ $ ; & ( ) | ^ < >

For example, the dollar sign (`$`) often indicates that the characters following it are a variable name. When used with the `echo` command, the program will attempt to retrieve the variable's value and display it. An example is shown in Listing 1.6.

## Listing 1.6: Using the echo command with a $ metacharacter

```
$ echo $SHELL
/bin/bash
$
$ echo It cost $1.00
It cost .00
$
```

Due to the `$` metacharacter, the `echo` command treats both the `$SHELL` and `$1` as variables. Since `$1` is not a variable in the second `echo` command and has no value, in its output `echo` displays nothing for `$1`. To fix this problem, you can employ *shell quoting*. Shell quoting allows you to use metacharacters as regular characters. To shell quote a single character, use the backslash (`\`) as shown in Listing 1.7.

## Listing 1.7: Using the echo command and shell quoting a single metacharacter

```
$ echo It cost \$1.00
It cost $1.00
$
$ echo Is Schrödinger\'s cat alive or dead\?
Is Schrödinger's cat alive or dead?
$
```

While the backslash is handy for shell quoting a single metacharacter, it can be tiresome when you have multiple ones. For several metacharacters, consider surrounding them with either single or double quotation marks, depending on the situation. A few examples of this shell quoting method are shown in Listing 1.8.

## Listing 1.8: Using the echo command and shell quoting multiple metacharacters

```
$ echo Is "Schrödinger's" cat alive or "dead"?
Is Schrödinger's cat alive or dead?
$
$ echo "Is Schrodinger's cat alive or dead?"
Is Schrodinger's cat alive or dead?
$
$ echo 'Is Schrodinger's cat alive or dead?'
> ^C
$
```

Notice the first two quoting methods work in Listing 1.8. The last one does *not* work. When the metacharacter to be shell quoted is a single quotation mark, you will need to employ another shell quoting method, such as the backslash or double quotation marks.

## Navigating the Directory Structure

Files on a Linux system are stored within a single directory structure, called a *virtual directory*. The virtual directory contains files from all the computer's storage devices and merges them into a single directory structure. This structure has a single base directory called the *root directory*, often simply called *root*.

When you log into the Linux system, your process's *current working directory* is your account's *home directory*. A current working directory is the directory your process is currently using within the virtual directory structure. Think of the current working directory as the room you are currently in within your home.

You can navigate through this virtual directory structure via the `cd` command. A helpful partner to `cd` is the `pwd` command. The `cd` command moves your working directory to a new location in the virtual directory structure, and the `pwd` program displays (prints) the current working directory. A few examples are shown in Listing 1.9.

## Listing 1.9: Using the cd and pwd commands

```
$ pwd
/home/Christine
$
$ cd /etc
$ pwd
/etc
$
```

> **NOTE** It is vital to know your current working directory, especially as you traverse the virtual structure. By default, many shell commands operate on the current working directory.

When using the `cd` command, you can employ either absolute or relative directory references. *Absolute directory references* always begin with a forward slash (`/`) in reference to the root directory and use the full name of the directory location. Listing 1.9 contains absolute directory references.

Relative directory references allow you to move with the `cd` command to a new position in the directory structure in relation to your current working directory using shorter directory arguments. Examples of relative moves are shown in Listing 1.10.

## Listing 1.10: Using the cd command with relative directory references

```
$ pwd
/etc
$ cd cups
$ pwd
/etc/cups
$ cd ..
$ pwd
/etc
$
```

In Listing 1.10 the current working directory is `/etc`. By issuing the `cd cups` command, which is a relative directory reference, you change the current working directory to `/etc/cups`. Notice that `pwd` always displays the directory using an absolute directory reference. The last command (`cd ..`) is also a relative move. The two dots (`..`) represent the directory above the current directory, which is the parent directory.

> You can also employ the single dot (`.`) directory reference, which refers to the current working directory. Although the single dot is not used with the `cd` command, it is commonly employed for tasks such as copying or moving files.

The `cd` command has several other shortcuts you can employ besides just the two dots. For example, to change your current working directory to your user account's home directory, use one of the following:

- cd
- cd ~
- cd $HOME

You can also quickly return to your most recent working directory by employing the `cd -` shortcut command. An example is shown in Listing 1.11.

## Listing 1.11: Using the cd - shortcut command

```
$ pwd
/etc
$
$ cd /var
$ pwd
/var
$
$ cd -
/etc
$ pwd
/etc
$
```

## Understanding Internal and External Commands

Within a shell, some commands that you type at the command line are part of (internal to) the shell program. These internal commands are sometimes called built-in commands. Other commands are external programs, because they are not part of the shell.

You can tell whether a command is an internal or external program via the `type` command. A few examples are shown in Listing 1.12.

## Listing 1.12: Using type to determine whether a command is external or internal

```
$ type echo
echo is a shell builtin
$
$ type pwd
pwd is a shell builtin
$
$ type uname
uname is /usr/bin/uname
$
```

Notice in Listing 1.12 that both the `echo` and `pwd` commands are internal (built-in) programs. However, the `uname` command is an external program, which is indicated by the `type` command displaying the `uname` program’s absolute directory reference within the virtual directory structure.

> A command may be available both internally and externally to the shell. In this case, it is important to know their differences, because they may produce slightly different results or require different options.

## Using Environment Variables

Environment variables track specific system information, such as the name of the user logged into the shell, the default home directory for the user, the search path the shell uses
