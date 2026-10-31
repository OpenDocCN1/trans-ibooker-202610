+   - **shell**: This command takes us to a shell prompt. We have already seen the use of this Meterpreter command in some of our previous recipes:

```
meterpreter > shell
Process 5704 created.
Channel 1 created.
Microsoft Windows [Version 6.1.7601]
Copyright (c) 2009 Microsoft Corporation.
All rights reserved.

C:\Windows\system32>
```

+   - **exit**: This command is used to terminate a Meterpreter session. It can also be used to terminate the shell session and return to Meterpreter.

These are a few useful system commands that can be used to explore the compromised target to gain more information about it. There are lots of other commands, which I am leaving for you to try and explore. You might have noticed how easy it is to use the Meterpreter commands and explore the target, which would have been a difficult task without it. In our next recipe, we will focus on some advanced Meterpreter commands.

# How it works...

Meterpreter works like any command interpreter. It is designed to understand and respond to various parameter calls through commands. It resides in the context of an exploited/compromised process and creates a client/server communication system with the penetration tester’s machine, as shown in the following diagram:

![](bbox=[0.2, 0.7, 0.8, 0.82])