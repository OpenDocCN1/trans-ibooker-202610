intranet web pages, a Linux system offers the following additional utilities and files to help with communication:

+   - **/etc/issue**: Contains text to be displayed on the tty terminal login screens (prior to logging into the system).
+   - **/etc/issue.net**: Contains logon screen messages for remote logins.
+   - **/etc/motd**: Called the Message of the Day file, contains text that is displayed after a user has logged into a tty terminal.
+   - **/bin/notify-send** (or **/usr/bin/notify-send**): Sends messages to a user employing the GUI but who is not logged into a tty terminal or does not have a GUI terminal emulator open.
+   - **/bin/wall** (or **/usr/bin/wall**): Sends messages (called *wall messages*) to users logged into a tty terminal or users who have a GUI terminal emulator open and have their message status set to “yes.”

The method(s) you choose depend on which one(s) best meet your company’s communication policies and needs.

By default, the systemctl utility (covered earlier in this chapter) will send a wall message when any of its following commands are issued: emergency, halt, power-off, reboot, or rescue. To prevent a wall message from being sent while using systemctl, include the --no-wall option in its command line.

The wall command sends simple messages to certain system users—those who are currently logged into a terminal (tty#) or a terminal-emulator (pts/#) and have their message status set to “yes.” To check your own message status, you can employ the mesg command as shown in Listing 5.29.

# Listing 5.29: Viewing your message status with the mesg command

```
$ msg
is y
$
```

Notice from the previous example, the mesg command shows the current message status. You can issue the mesg y command to turn on messaging and mesg n to turn it off.

To see who is currently logged into the system and whether or not they have their message status set to “yes,” use the who -T command. All users who can receive wall messages will have a plus (+) following their username.

Figure 5.3 and Figure 5.4 show the wall command in action. Notice in Figure 5.3 that the message is written after the wall command is issued. You enter the message and then press the Ctrl+d key combination to send the communication.