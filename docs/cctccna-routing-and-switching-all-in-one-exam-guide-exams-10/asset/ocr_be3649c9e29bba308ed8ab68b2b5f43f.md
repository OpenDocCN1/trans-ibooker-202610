# IOS Basics

The next few sections describe some of the features built into the CLI that will make your configuration and management tasks easier. These features include how to abbreviate commands, how to bring up detailed help on commands and their specific parameters, and how to decipher the output of commands, recall commands, and edit commands.

# Command Abbreviation and Completion

The CLI of IOS enables you to abbreviate commands and parameters to their most unique characters. This feature is useful for those who are physically challenged at typing. As an example, you could type **en** instead of **enable** when you want to go from user EXEC mode to privileged EXEC mode, like this:

```
IOS> en
IOS#
```

The Cisco device, internally, completes the command for you. However, the characters that you enter must make the command unique. As an example, you couldn’t type just the letter e for **enable**, since other commands being with the letter e, such as **exit**.

# Context-Sensitive Help

One of the more powerful features of IOS is context-sensitive help. Context-sensitive help is supported at all modes within IOS, including user EXEC, privileged EXEC, and configuration modes. You can use this feature in a variety of ways. If you are not sure what command you need to execute, at the prompt, type either **help** or **?**. The Cisco device then displays a list of commands that can be executed at the level in which you are currently located, along with a brief description of each command. Here is an example from a router’s CLI at user EXEC mode: