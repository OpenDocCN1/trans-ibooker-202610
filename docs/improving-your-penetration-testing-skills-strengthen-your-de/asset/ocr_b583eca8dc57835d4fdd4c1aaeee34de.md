```
Background channel 2? [y/N] y
meterpreter >
```

As you can see, our channel, 2, was a command-prompt channel, so by using the channel command with -i followed by the channel number, we are directly dropped into the command-prompt mode from where we can execute system commands.

+   5. To background a channel, use Ctrl + Z. We can easily switch between channels by using the channel command. In order to end a channel, we can use the channel command followed by -c and the channel ID:

```
meterpreter > channel -c 2
[*] Closed channel 2.
```

This recipe demonstrates the power of using multiple channels. It also shows how easy it is to manage them simultaneously and switch between different channels. The use of channels becomes important when we are running multiple services on the target machine.

# How it works...

Metasploit tags each message with a separate channel ID, which helps it in identifying the channel context in which the particular command should be executed. As stated earlier, the communication process in Meterpreter follows the TLV protocol, which gives the flexibility of tagging different messages with specific channel IDs in order to provide multichannel communication support.

# Meterpreter anti-forensics

In the previous recipe, we read about some of the important and useful Meterpreter file system commands that can be used to perform various tasks on the target machine. Meterpreter contains another interesting command called timestomp. This command is used to change the Modified-Accessed-Created-Entry (MACE) attributes of a file. The attribute value represents the date and time when any of the MACE activities occur within the file. Using the timestomp command, we can change these values.