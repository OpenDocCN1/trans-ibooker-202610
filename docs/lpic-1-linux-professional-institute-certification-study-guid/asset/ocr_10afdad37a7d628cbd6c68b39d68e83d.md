The test environment variable set in the child shell doesn’t exist when we go back to the parent shell.

# # Setting Global Environment Variables

Global environment variables are visible from any child processes created by the process that sets the global environment variable. The method used to create a global environment variable is to create a local environment variable and then export it to the global environment. This is done by using the export command:

```
$ echo $test
testing a long string
$ export test
$ bash
$ echo $test
testing a long string
$
```

After exporting the local environment variable test, we started a new shell process and viewed the value of the test environment variable. This time, the environment variable kept its value, as the export command made it global.

> > Notice that when exporting a local environment variable, you don't use the dollar sign to reference the variable's name.

# # Locating System Environment Variables

The Linux system uses environment variables to identify itself in programs and scripts. This provides a convenient way to obtain system information for your programs. The trick is in how these environment variables are set.

When you start a Bash shell by logging into the Linux system, by default Bash checks several files for commands. These files are called startup files. The startup files Bash processes depend on the method you use to start the Bash shell. There are three ways of starting a Bash shell:

+   - As a default login shell at login time
+   - As an interactive shell that is not the login shell
+   - As a noninteractive shell to run a script

The following sections describe the startup files the Bash shell executes in each of these startup methods.