Notice that this system employs both system and user journal files and that several of the files have been archived. The systemd-journald configuration dictates when the archived journal files are removed from the system based on size directives, such as SystemMaxFileSize, or time directives, such as MaxFileSec.

On some systems, you can manually rotate (archive) active journal files. If this feature is available on your system, you can do this via the journalctl --rotate command.

# Layering Your Logging

If desired (or required), you can have both systemd-journald and a syslog protocol application, such as rsyslog, running and working together. There are two primary ways to accomplish this:

**Journal Client Method** This method allows a syslog protocol program to act as a *journal client*, reading entries stored in the journal file(s). It is typically the preferred way, because it avoids losing any important messages that may occur during the system boot, before the syslog service starts. Also for rsyslog, this is commonly already configured, which is handy.

For rsyslog, if this method is not already configured or you'd like to check your system, look in the /etc/rsyslog.conf file. It needs to have the imuxsock and/or imjournal module being loaded via ModLoad without a preceding pound sign (#), as shown here:

```
$ grep ModLoad /etc/rsyslog.conf | grep -E "imjournal | imuxsock"
$ModLoad imuxsock # provides support for local system logging [...]
$ModLoad imjournal # provides access to the systemd journal
$
```

**Forward to Syslog Method** This method employs the file /run/systemd/journal/syslog. Messages are forwarded to the file (called a socket) where a syslog protocol program can read them.

To use this method, you need to modify the journal configuration file, /etc/systemd/journald.conf, and set the ForwardToSyslog directive to yes. Keep in mind that you'll need to load the modified journal.conf file into systemd-journald in order for it to take effect. Unfortunately, you cannot employ the systemctl reload option to load the new configuration for systemd-journald. Instead, using super user privileges, you must restart the service:

```
systemctl restart systemd-journald
```

# Making the Journal Persistent

On some distributions, the journal entries are stored in the /run/log/journal directory, which is *volatile*, meaning it is removed, and the journal entries are lost whenever the system is shut down. Therefore, typically system admins set the Storage directive within the journald.conf file, changing it to persistent.