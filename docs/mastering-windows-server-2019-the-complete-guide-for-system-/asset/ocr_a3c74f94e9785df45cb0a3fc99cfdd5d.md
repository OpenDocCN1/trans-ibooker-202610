Both shutdown and Restart-Computer are useful for shutting down or restarting a server. On my own computer, these commands are most commonly preceded by the hostname command. When rebooting a server, you want to take special care that you restart the correct machine, so I find it best to open a PowerShell prompt, do a quick hostname check and then run a Restart command from that same prompt. This ensures that I am restarting the server that was returned in the hostname output:

```
shutdown /r /t 0
```

If you run a simple shutdown command, the system will shut down in one minute. I'm not sure why this is the default, as I have never found any IT administrator who actually wanted to wait that extra minute before shutting down their system. Instead, it is more efficient to set a time limit before that shutdown commences. In the preceding command, I have told the shutdown command that I want to restart instead of shutting down; that is what /r does. I have also told it to wait zero seconds before performing this restart. This way, it happens immediately; I don't have to wait for that default 60-second timer.

# Query user or quser

Most useful in RDS environments, the quser command will display all of the users that are currently logged in to a server, including statistics about whether they are logged in locally or remotely, and how long their session has been active. I commonly use quser to discover RDS sessions that have been sitting around idle for a long time, so that I can end those sessions and free up resources on the server:

![](bbox=[0.13, 0.51, 0.87, 0.62])

Figure 11.10: The quser command displays logged-in sessions

```
quser /server:WEB1
```

Using quser in combination with the /server switch allows you to see the currently logged-in users on a remote system. This way, you can remain logged in to a single server in your RDS farm but can check on the user sessions for all of your systems without having to log into them. You could even write a script that runs this command against each of your session host servers and outputs the data to a file. This output could then be run on a schedule and used as a reporting mechanism for keeping track of which users were logged in to which RDS session host servers at any given time.