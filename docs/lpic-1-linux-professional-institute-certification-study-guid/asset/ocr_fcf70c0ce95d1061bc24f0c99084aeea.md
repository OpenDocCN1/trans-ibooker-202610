Now that you've seen the basics for creating and running shell scripts, the next section dives into some more advanced features you can add to make fancier shell scripts.

# Advanced Shell Scripting

The previous section walked through the basics of how to group normal command-line commands together in a shell script file to run in the Linux shell. This section adds to that by showing more features available in shell scripts to make them look and act more like real programs.

# Displaying Messages

When you string commands together in a shell script file, the output may be somewhat confusing to look at. It would help to be able to customize the output by separating the output and adding our own text between the output from the listed commands.

The echo command allows you to display text messages from the command line. When used at the command line, it's not too exciting:

```
$ echo This is a test
This is a test
$
```

But with echo, you can now insert messages anywhere in the output from the shell script file. Listing 9.7 demonstrates how this is done.

# Listing 9.7: Displaying messages from shell scripts

```
$ cat test1.sh
#!/bin/bash
# This script displays the date and who's logged in
echo The current date and time is:
date
echo
echo "Let's see who's logged into the system:"
who
$ ./test1.sh
The current date and time is:
Thu Feb 20 19:55:44 EST 2019

Let's see who's logged into the system:
rich   :0           2019-02-20 19:15 (:0)
$
```