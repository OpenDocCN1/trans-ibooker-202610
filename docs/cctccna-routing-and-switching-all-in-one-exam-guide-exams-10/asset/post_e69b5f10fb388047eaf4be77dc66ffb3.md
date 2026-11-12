
running.

9. A. The IOS image file is stored in flash memory.

## Performance-based Answers

1. The following is the mapping of commands with their definition.
Keep in mind that on the real exam you will need to drag items on the left side and drop them on the matching item on the right side.

![](img/0eafdcebe219ef3756f0e57bcf1eb997_303_0.png)

2. The following is the correct order of events when the Cisco device is started. Keep in mind that on the real exam you will need to drag items found on the left side and drop them into order on the right side.

## Steps

- 1. Device loads and runs POST
- 2. Bootstrap program loaded and executed
- 3. Configuration register value read to determine location of IOS image and configuration file
- 4. IOS is loaded from flash
- 5. Startup config is applied

## Cisco IOS Basics

In this chapter, you will

- Learn about interacting with the command-line interface (CLI)
- Get familiar with the basics of IOS
- Learn about basic IOS configuration
- Learn about basic IOS security configuration
- Learn some IOS operation and verification commands

This book emphasizes the fundamental and important concepts of accessing, configuring, and managing Cisco routers and switches through the command-line interface (CLI) and the console port. You’ll use commands and the console port connection to perform basic configuration of Cisco routers and switches.

These discussions assume that you have never configured a Cisco router or switch; we therefore begin with the basics—the operating system (OS) used by these devices, the advantages provided by Cisco’s OS, and the use of commands to configure a Cisco device. The chapters that follow will continue the discussion by exploring configuration of Cisco switches, routing features, and security features.

## Command-Line Interface

How you access IOS CLI on a Cisco device for the first time depends on the kind of device you are configuring. In almost every case, you will use the console interface initially to interact with the device; however, gaining access to the CLI from the console port can be different from one device to another. On a Cisco router, for instance, you are taken directly to IOS CLI
when you log in from the console port. If IOS cannot find a configuration file for the router or switch, IOS takes you through Setup mode (via the System Configuration Dialog), which is a basic configuration script that prompts you for information about how you want to configure your IOS device (discussed in Chapter 6).

Once you have configured your Cisco device via the console port, you can then use other methods of accessing and changing its configuration, such as telnet or Secure Shell (SSH)—Cisco calls this *virtual type terminal* (VTY) access. Or you can use Trivial File Transfer Protocol (TFTP), File Transfer Protocol (FTP), Simple Network Management Protocol (SNMP), or a web-based solution (Cisco Configuration Professional, or CCP, for routers). A variety of Cisco management products can be used to configure and manage your Cisco device, such as the Cisco Security Manager (CSM); however, this book focuses primarily on IOS CLI.

## IOS Differences

In this and subsequent chapters, you will see that even though both Cisco routers and switches run IOS, the commands used by these products are frequently different! In other words, how you configure a feature on a Cisco router may be different from how you configure the same feature on a Catalyst switch. However, how you access IOS and maneuver around IOS access levels, as well as how you use many of the management commands, is the same on *all* IOS products. This can be confusing to a Cisco novice—one command for a particular feature is the same on all Cisco products, but configuring another feature is different between a Cisco router and a Catalyst switch.

In addition to command differences between different products, such as routers and switches, command differences may exist within a product line, such as Cisco routers, for instance. For example, Cisco sells different flavors of its IOS software for routers, depending on the features that you need. Because of hardware differences, some commands work on some routers but not on others.

![](img/0eafdcebe219ef3756f0e57bcf1eb997_307_0.png)

**NOTE** Lately, the number of IOS implementations has greatly diminished. Each version of IOS may have had 5, 10, or even 15 or more different packages you’d need to download, based on what features you purchased. Today, the number of packages is smaller, typically 3 or fewer. Instead, Cisco bundles most features in an IOS package but requires you to purchase a license key to unlock those features. Licenses are discussed in Chapter 8.

## Interacting with IOS

The CLI is a character- or text-based interface. To interact with the CLI, you simply need to type in commands, just as you would do when typing an essay in a text editor or a message in an e-mail program. You can even use functions such as copy-and-paste with IOS CLI: so, for example, you can copy the complete configuration from a router using a terminal emulator’s copy function, paste this into a text editor, make changes to the configuration, select and copy the new configuration, and then paste all of these commands back into the CLI.

The CLI supports a command parser. Whenever you press the ENTER key, IOS parses the command and parameters that you entered and checks for correct syntax and options. When you paste multiple commands into the CLI, IOS still performs this process for each command that is included in the paste function. If you make a mistake with one command, the CLI parser will display an error message, but it will continue with the next command in the pasted list.

![](img/0eafdcebe219ef3756f0e57bcf1eb997_307_1.png)

**TIP** Before making any configuration changes on your IOS device, you should first back it up! You’ll learn how to do this across the network in Chapter 8, but you could easily do this by executing the **show running-config** command, copying the output configuration, and pasting it into a backup text file on your computer. The backup text file can then be referenced should something go wrong with a configuration change. You will learn more about backing up and restoring your configuration in Chapter 8.

## CLI Access Modes

Each Cisco device supports several access modes. For CLI interaction, four modes are supported:

- **User EXEC** Provides basic access to IOS with limited command availability (basically simple monitoring and troubleshooting commands)
- **Privileged EXEC** Provides high-level management access to IOS, including all commands available at user EXEC mode
- **Global Configuration** Enables configuration changes to be made to the device
- **ROM Monitor (ROMMON)** Loads a bootstrap program that allows for low-level diagnostic testing of an IOS device, performing the password recovery procedure, and performing an emergency upgrade

Of the four modes, the first three apply to IOS. While in ROMMON mode, IOS has not loaded and therefore packets are not moved between interfaces of the device. Both EXEC modes can be password-protected, enabling you to limit who can access your device to perform management, configuration, and troubleshooting tasks. The next two sections introduce the two EXEC modes.

## User EXEC Mode

Your initial access to the CLI is via the user EXEC mode, which has only a limited number of IOS commands you can execute. Depending on the Cisco device’s configuration, you may be prompted for a password to access this mode. User EXEC mode is typically used for basic troubleshooting of networking problems. You can tell that you are in user EXEC mode by examining the prompt on the left side of the screen:

```
IOS>
```

If you see a > character at the end of the line, you know that you are in user EXEC mode. The information preceding the > is the name of the Cisco device. For instance, the default name of all Cisco routers is *Router*, whereas the 2960 switch’s user EXEC prompt looks like this: **Switch>**. These device names can be changed with the **hostname** command, which is discussed later in this chapter. In the preceding example, I renamed the device *IOS*.

## Privileged EXEC Mode

Once you have gained access to user EXEC mode, you can use the **enable** command to access privileged EXEC mode:

```
IOS> enable
IOS#
```

Once you enter the **enable** command, if a privileged EXEC password has been configured on the Cisco device, you will be prompted to enter it. Upon successfully authenticating, you will be in privileged EXEC mode. You can tell that you are in this mode by examining the CLI prompt—in the preceding code example, notice that the > changed to a #.

When you are in privileged EXEC mode, you have access to all of the user EXEC commands as well as many more advanced management and troubleshooting commands. These commands include extended ping and trace abilities, the ability to manage configuration files and IOS images, and the ability to perform detailed troubleshooting using **debug** commands. About the only thing that you can’t do from this mode is change the configuration of the Cisco device—this can be done only from configuration mode.

To return to user EXEC mode from privileged EXEC mode, use the **disable** command:
