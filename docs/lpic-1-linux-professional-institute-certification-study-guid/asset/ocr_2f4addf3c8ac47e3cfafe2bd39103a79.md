| Directive | Description |
|---|---|
| ForwardToWall= | Set to yes or no. If yes, any received messages are forwarded as wall messages to all users currently logged into the system. (Default is yes.) |
| MaxFileSec= | Set to a number followed by a time unit (such as month, week, or day) that sets the amount of time before a journal file is rotated (archived). Typically this is not needed if a size limitation is employed. To turn this feature off, set the number to 0 with no time unit. (Default is 1month.) |
| RuntimeKeepFree= | Set to a number followed by a unit (such as K, M, or G) that sets the amount of disk space systemd-journald must keep free for other disk usages when employing volatile storage. (Default is 15% of current space.) |
| RuntimeMaxFileSize= | Set to a number followed by a unit (such as K, M, or G) that sets the amount of disk space systemd-journald journal files can consume if it is volatile. |
| RuntimeMaxUse= | Set to a number followed by a unit (such as K, M, or G) that sets the amount of disk space systemd-journald can consume when employing volatile storage. (Default is 10% of current space.) |
| SystemKeepFree= | Set to a number followed by a unit (such as K, M, or G) that sets the amount of disk space systemd-journald must keep free for other disk usages when employing persistent storage. (Default is 15% of current space. |
| SystemMaxFileSize= | Set to a number followed by a unit (such as K, M, or G) that sets the amount of disk space systemd-journald journal files can consume if it is persistent. |
| SystemMaxUse= | Set to a number followed by a unit (such as K, M, or G) that sets the amount of disk space systemd-journald can consume when employing persistent storage. (Default is 10% of current space.) |

Changing a journal file from being volatile to persistent is covered later in this chapter. Thus, the Storage directive settings in Table 7.11 need a little more explanation, because they are involved in this activity:

+   auto: Causes systemd-journald to look for the /var/log/journal directory and store event messages there. If that directory doesn't exist, it stores the event messages in the temporary /run/log/journal directory, which is deleted when the system shuts down.

+   persistent: Causes systemd-journald to automatically create the /var/log/journal directory if it doesn't currently exist and store event messages there.