
![](img/0eafdcebe219ef3756f0e57bcf1eb997_431_0.png)

**EXAM TIP** Remember for the Cisco CCT exam that the default configuration register value is *0x2102*, which causes a router to boot up using its default bootup process. (Look for **boot system** commands in the startup configuration, and if none are found, load the first IOS in flash, and then load the default startup configuration file.) You can see the configuration register value with the **show version** command. If you’ve changed this value, you will see the existing value and the value the router will use upon rebooting. If the router boots up and doesn’t have a configuration, but one exists in NVRAM, check the router’s configuration register to see if it is set to *0x2142*: this register setting causes IOS to ignore any configuration file in NVRAM when booting.

## Changing the Configuration Register from ROM Monitor

Of course, one problem with the configuration mode method of changing the register value is that you must gain access to Privileged EXEC mode first. This can be a problem if you don’t know the passwords on the router. A second method, though, enables you to change the register value without having to log into the router. To use this method, you’ll need console access to the router—you can’t do this from the auxiliary line or from a VTY session. Next, you’ll turn off the router and then turn it back on. As the router starts booting, you’ll break into ROMMON mode with the router’s break sequence. To break into the router, once you see the bootstrap program has loaded, you can, in most cases, use the `CTRL-BREAK` control sequence to break into ROMMON mode. Note that this control sequence may differ, depending on the terminal emulation program you are using on your PC.

Once you’re in ROMMON mode, you can begin the process of changing the register value using one of two methods, depending on the router model that you have. Cisco devices typically use the **confreg** command to modify the configuration register from ROMMON. You can use the command by following it with the value you want to change the configuration register to, as shown here:

```
rommon 4 > confreg 0x2142
```

Or you can use it in script mode, where it prompts you about how you want to set the setting. This script asks you basic questions about the function and bootup process of the router. What’s nice about the script is that you don’t need to know the hexadecimal values for the configuration register, since the router will create them for you as you answer these questions. Here is an example of using this script:

```
rommon 5 > confreg
    Configuration Summary
enabled are:
load rom after netboot fails
console baud: 9600
boot: image specified by the boot system commands
    or default to: cisco2-C3600

do you wish to change the configuration? y/n  [n]:  y
enable  "diagnostic mode"? y/n  [n]:
enable  "use net in IP bcast address"? y/n  [n]:
disable "load rom after netboot fails"? y/n  [n]:
enable  "use all zero broadcast"? y/n  [n]:
enable  "break/abort has effect"? y/n  [n]:
enable  "ignore system config info"? y/n  [n]:
change console baud rate? y/n  [n]:
change the boot characteristics? y/n  [n]:

    Configuration Summary enabled are:
load rom after netboot fails
console baud: 9600
boot: image specified by the boot system commands
    or default to: cisco2-C3600
do you wish to change the configuration? y/n  [n]:  n
rommon 6 >
```

![](img/0eafdcebe219ef3756f0e57bcf1eb997_432_0.png)

**TIP** As a shortcut, you could also execute the **confreg 0x2142** command in ROMMON mode.

Just as in the System Configuration dialog, any information in brackets ([ ]) represents default values. The first question you’re asked is whether you want to change the configuration, which means change the register: answer y to continue. If you answer y to Ignore System Config Info?, the third hexadecimal digit becomes 4, making a router’s register value appear as 0x2142. This option is used when you want to perform the password recovery procedure. For the next-to-last question, Change The Boot Characteristics?, if you answer y, it will repeat the questions again. Answer n to exit the script. If you make any changes, you are asked to save them (Do You Wish To Change The Configuration?)—answer y to save your new register value. Once you are done changing the register, reboot the router. On many routers, just type i or b in ROMMON mode to boot it up.

## Password Recovery

One of the times you may want to manipulate the configuration registers is when you have forgotten the password for your Cisco device and need to bypass the password to be able to administer the device. The passwords are stored in your startup configuration, which is stored in NVRAM. To bypass the password you need to modify the configuration registers so that your device boots without loading the startup configuration (ignore NVRAM). Once the device has booted without your startup configuration, you will be able to gain access without a password and can then set the password to a new value.

To configure your Cisco device to ignore NVRAM, you will need to enable bit 6, which has a value of 4, resulting in the configuration register value of 0x2142 instead of the default 0x2102.

Following are the steps to recover the password on your Cisco device:

1. Use the show version command to view the current configuration register value. It is most likely 0x2102.

2. When you power on the Cisco router, press CTRL-BREAK to disrupt the boot process and go into ROMMON mode. You should see the rommon> prompt.

3. Type the following command to configure the device to skip startup config when booting:

   ```
   rommon>confreg 0x2142
   ```

4. Reboot the router with the **reset** command.

5. When the router reboots, it will not load the startup configuration and ask if you want to use the System Configuration dialog. Type **No** and press ENTER.

6. At the **router>** prompt, enter the **enable** command to get into privileged EXEC mode. Notice you are not asked for a password:

   ```
   Router>enable
   Router#
   ```

7. Use the **copy startup-config running-config** command to copy the startup configuration that was not read during bootup to be your current configuration. This is so you do not lose any settings.

8. View the configuration with the **show running-config** command.

9. Change the password by using the **enable password <newpass>** or **enable secret <newsecret>** command within global configuration mode. You can also remove the password by using the **no enable password** or **no enable secret** command.

10. Copy that from your current configuration to your startup configuration by using the **copy running-config startup-config** command (or use the **write** command).

11. Use the **config-register 0x2102** command to change the configuration registers back to the default value of *0x2102* so that NVRAM is read during bootup.

12. Reboot the router with the **reload** command.

![](img/0eafdcebe219ef3756f0e57bcf1eb997_434_0.png)

**EXAM TIP** Remember for the CCT exam how to perform password recovery procedures. You need to break into ROMMON mode, change the configuration register value to *0x2142*, and reboot the router.

## Exercise 8-5: Using the Password Recovery Procedure

In this exercise you will pretend that you forget the password for the VAN-R1 router. Note that the enable password is configured as *enablepass*, and the secret is set as *secretpass* from the last exercise. This exercise assumes the same network configuration as the previous exercises in this chapter.

1. Console into VAN-R1 and navigate to privileged EXEC mode.

2. Use the **show version** command to view the current configuration register value. Record it here: ______________.

3. Power off the Cisco router.

4. Power on the Cisco router, press CTRL-BREAK to disrupt the boot process, and go into ROMMON mode. You should see the **rommon>** prompt.

5. Type the following command to configure the device to skip startup config when booting:

   ```
   rommon>confreg 0x2142
   ```

6. Reboot the router with the **reset** command.

7. When the router reboots, it will not load the startup configuration and ask you to use the System Configuration dialog. Type **No** and press ENTER.

8. At the **router>** prompt, enter the **enable** command to get into privileged EXEC mode. Notice you are not asked for a password:

   ```
   Router>enable
   Router#
   ```

9. Use the **copy startup-config running-config** command to copy the startup configuration that was not read during bootup to be your current configuration. This is so you do not lose any settings.

10. View the configuration with the **show running-config** command.

11. Change the enable password by using the **enable password labpass** global configuration command.
