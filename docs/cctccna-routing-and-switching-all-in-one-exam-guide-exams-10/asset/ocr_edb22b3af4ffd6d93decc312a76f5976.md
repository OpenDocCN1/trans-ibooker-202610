This error indicates that you have not entered all the necessary parameters for the command. The syntax of the command is correct, but more parameters are necessary. In this case, you can use the context-sensitive help feature to help you figure out what parameter or parameters you forgot.

You’ll see an error message similar to the following if you do not type in enough characters to make a command or parameter unique:

```
% Ambiguous command: "show i"
```

In this example, apparently, more than one parameter for the **show** command begins with the letter i. Again, you can use context-sensitive help to figure out what parameter to use:

```
Router# show i?
idb  interfaces  ip  ipv6
Router# show i
```

If you enter a command that IOS does not understand, you’ll see this error message:

```
% Unknown command or computer name, or unable to find computer address
```

If you see this, use the context-sensitive help to figure out the correct command to enter.

![](bbox=[0.14, 0.61, 0.24, 0.68])

**EXAM TIP** Don’t be afraid to take advantage of the context-sensitive help feature during the simulation and simlet questions in the exam. This feature can be used for help with entering commands, and it is available in the router and switch simulation and simlet questions in the exam; use it.

# Entering Commands

Four key features included in IOS relate to entering commands: