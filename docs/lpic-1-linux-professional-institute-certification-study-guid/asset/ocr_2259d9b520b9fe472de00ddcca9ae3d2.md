> When powering off a system, after the processes are stopped and the CPU is shut down, signals are sent to the hardware telling the various components to power down. For operating systems using Advanced Configuration and Power Interface (ACPI)–compliant chipsets, these are ACPI signals. These special communications are handled by the ACPI daemon, acpid. This daemon manages signals sent to various hardware devices via predefined settings for particular events, such as pressing the system’s power button or closing a laptop system’s lid.

The utility with the most flexibility in rebooting or powering off your system is the shutdown command. Its basic syntax is as follows:

```
shutdown [OPTIONS...] TIME [WALL-MESSAGE]
```

The [OPTIONS] include switches to halt the system (-H), power off the system (-P), and reboot the system (-r), as well as several other useful selections. After you’ve started a shutdown process, you can typically cancel it using the shutdown -c command. See the man pages for additional shutdown options you may desire to use.

> If no [OPTIONS] are used, the shutdown command performs differently depending on the distribution you are using.

The TIME parameter allows you to specify a time to enact the shutdown options. It takes many formats, such as a military time layout specified as hh:mm. You can indicate the number of minutes from the current system time using a +n or n format. The shutdown command allows the now time parameter to indicate 0 minutes from now (immediately). On some distributions, if TIME is not specified, a +1 is assumed. See the man pages for all the TIME specifications available on your distribution.

The [WALL-MESSAGE] parameter lets you modify the shutdown command message sent to any logged-in users. Wall messaging is covered in the next section.

> For any utility used to shut down the system, the processes are sent a SIGTERM signal (covered in Chapter 2). This allows the various running programs to close their files properly and gracefully terminate. However, in unusual cases, you may have a situation where a process refuses to shut down. You can use the lsof -p PID command to see if the running program has any files open. If not, then you can attempt to use the kill -9 PID command. However, always tread cautiously in these cases.

# Notifying the Users

When you perform any function that changes the system’s state for logged-in users, it’s a good idea to let them know ahead of time so that they can wrap up any work before being kicked out. Besides the old standbys of email, automated text messaging, and company