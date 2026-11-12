is discussed in Chapter 8.) On both routers and switches, you use the **hostname** command to change the name of the device. Here is a simple example of changing the name *Router* to the name *Perimeter*:

```
Router(config)# hostname Perimeter
Perimeter(config)#
```

First, notice that the new name of the device (*Perimeter*) is placed after the **hostname** command. As soon as you press ENTER, the new CLI prompt is immediately changed—it contains the device’s new name.

To undo changes or negate a command on a Cisco device, you can precede the command with the **no** parameter. As an example, to change the hostname back to the factory default, use this command:

```
Perimeter(config)# no hostname Perimeter
Router(config)#
```

In certain cases, you don’t have to include the parameters of the command when negating it. In the preceding example, you could easily have typed in **no hostname** to accomplish the change, but in other cases, you must type in **no** and follow it with the entire configuration command.

![](bbox=[0.14, 0.56, 0.24, 0.63])

**EXAM TIP** For the Cisco CCT certification, remember that the **hostname** command assigns a locally significant identifier to IOS device. In some of the exam simulation questions, you may be required to configure the hostname as part of your configuration. On the exam, the name will be case-sensitive!

![](bbox=[0.14, 0.8, 0.22, 0.86])