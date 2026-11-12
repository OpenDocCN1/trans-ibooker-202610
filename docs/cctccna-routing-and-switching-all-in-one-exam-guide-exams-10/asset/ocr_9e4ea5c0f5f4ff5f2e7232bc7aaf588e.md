command. Also know that if you misconfigured the **boot system** command, the router may not be able to find and load an IOS image.

# Changing the Configuration Register from Configuration Mode

You can change the configuration register value from configuration mode or from ROMMON mode. If you already have privileged EXEC access to the router and want to change the register value, use this command:

```
Router(config)# config-register 0xhexadecimal_value
```

![](bbox=[0.14, 0.37, 0.24, 0.43])

**TIP** If you configured your router, rebooted it, and it came up with the System Configuration dialog prompt, then you either forgot to save your running configuration before rebooting or misconfigured the configuration register.

The register value is four hexadecimal digits, or 16 bits, in length. Each bit position in the register, though, indicates a function that the bootstrap program should take. Therefore, you should be very careful when configuring this value on your router.

When entering the register value, you must always precede it with 0x, indicating that this is a hexadecimal value. If you don’t do this, the router assumes the value is decimal and converts it to hexadecimal. On Cisco routers, the default configuration register value is 0x2102, which causes the router to use the default bootup process in finding and locating IOS images and configuration files. If you change this to 0x2142, this tells the bootstrap program that, upon the next reboot, it should locate IOS using the default behavior, but it should not load the configuration file in NVRAM; instead, you are taken directly into the System Configuration dialog. This is the value that you will use to perform the password recovery procedure.