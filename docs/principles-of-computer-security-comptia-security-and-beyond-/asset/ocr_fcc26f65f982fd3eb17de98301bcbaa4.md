b. Click the CLI tab at the top of the Switch0 window. If necessary, press ENTER until you see the Switch> prompt, which indicates that you are in user EXEC mode. In user EXEC mode, you can look at certain status information, but not much else. Type enable and press ENTER at the prompt to go from user EXEC mode to privileged EXEC mode (also called “enable mode” because of the command to get there). You’ll notice the prompt changes to Switch#. In privileged EXEC mode, you can copy, erase, configure, and display settings. Type configure terminal and press ENTER at the prompt to go to global configuration mode. You’ll notice the prompt changes to Switch(config)#. In global configuration mode, you can configure many items, including the clock, hostname, and passwords.

In the Cisco IOS (Internetworking Operating System), you only need to type in enough of a command to uniquely identify it from other commands in the same mode; therefore, en would be the same as ena, enab, enabl, and enable. Also, you can type in one or more characters and press TAB to see the command autocomplete. Furthermore, if you enter in a few character and type ?, the Cisco IOS will show you ways to complete the command. If you press the spacebar before the ? symbol, the Cisco IOS will show you what it is expecting after the current entry on the prompt.

c. Give the switch a hostname, which will appear on the prompt, by typing hostname followed by the hostname. In this case, use the hostname S0 (for Switch0).

d. Type END and press ENTER to go back to privileged EXEC mode. If you see output in the console, press ENTER again to get a prompt. To go down one mode at a time, type exit and press ENTER. To go all the way down to privileged EXEC mode from a higher mode, type END and press ENTER or just press CTRL-Z.

e. Using the preceding steps, give the second switch a hostname of S1 (for Switch1).

f. Open a command prompt on one PC by clicking it, selecting the Desktop tab, and then selecting Command Prompt.

g. From each PC, ping the other three.