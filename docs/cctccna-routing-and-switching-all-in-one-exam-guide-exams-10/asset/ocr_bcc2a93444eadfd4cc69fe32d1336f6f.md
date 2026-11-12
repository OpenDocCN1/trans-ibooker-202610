configuration mode you are currently working in. Here is an example of going from a subconfiguration mode to Global Configuration mode:

```
IOS(config)# line con 0
IOS(config-line)# exit
IOS(config)#
```

In many cases, when you are working in a subconfiguration mode and type in a Global Configuration mode command, IOS executes it and the mode changes to Global Configuration mode, like this:

```
IOS(config)# line con 0
IOS(config-line)# hostname 2960
2960(config)#
```

![](bbox=[0.14, 0.4, 0.24, 0.47])

EXAM TIP Remember that many Global Configuration mode commands affect the IOS device as a whole, such as the hostname command: you enter the command once and it affects the entire device. Subconfiguration mode commands affect only one specific component of an IOS device, such as an interface, a line, or a routing protocol, to name a few. Use the exit command to go back one configuration level. Use the end command or press CTRL-Z to exit configuration mode.

Notice that when the hostname command was executed in line subconfiguration mode, the switch changed its name as well as the mode. Sometimes, you’ll get an invalid command response from the router; in that case you’ll have to exit out of subconfiguration mode before executing the command in global configuration.

# Line Configuration

Lines on IOS devices provide access to an EXEC shell; sometimes this is called character mode access, since it deals with a CLI. Table 7-3 shows the supported line types. The line command is used to reference a line in