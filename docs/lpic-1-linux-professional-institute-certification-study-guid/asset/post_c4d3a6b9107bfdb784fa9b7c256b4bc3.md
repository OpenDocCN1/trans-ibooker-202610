
### Summarize managing groups.

The commands involved in creating, modifying, and deleting groups are the groupadd, groupmod, and groupdel commands. These commands cause modifications to the /etc/group file. If you need to add a user to a group, you need to employ the usermod utility. A user can easily switch from the account's default group to another group in which the account is a member by using the newgrp program. Account group membership can be audited via the groups and getent commands as well as by viewing the /etc/group file.

### Explain how to use various email utilities.

Managing and troubleshooting the three MTAs (Sendmail, Postfix, Exim) can be accomplished using various available email utilities. To view an account's mail on the system, use the mail utility. For messages that may be stuck on the system, look at the mail queue using either the mailq or sendmail -bp command. You can set up email aliases by editing the /etc/aliases file and then running the newaliases command, which will update the aliases database, /etc/aliases.db. To temporarily forward emails, a user can create the .forward file in their home directory, put in the username to whom the emails should be sent, and then set the proper permissions on the file. Because the Sendmail MTA was so popular, Postfix provides a Sendmail emulation layer, which allows Sendmail commands to be used on their system.

### Clarify how rsyslogd logging operates.

The rsyslogd service implements the syslog protocol. It uses the /etc/rsyslog.conf configuration file, and on some distributions includes the /etc/rsyslog.d/*.conf files. Within the configuration files, rules determine what to log and where to log it via the facility.priority action format. Log messages can be sent to a remote logging server (with or without compression) using the TCP|UDP([z])HOST[:PORT] format. Log files are rotated with the logrotate cron job, which is configured in the /etc/logrotate.conf file. Log messages can be generated manually using the logger command.

### Summarize how to review and manage journal entries.

The systemd-journald service is responsible for journal message data. The daemon is controlled via the /etc/systemd/journal.conf configuration file. By default the journal files are stored in the /run/systemd/journal/ directory tree, which makes them volatile. To make the files persistent and store them in the /var/log/journal/ directory tree, the Storage directive within the configuration file is changed to persistent and the systemd-journald service restarted. The rsyslog utility can act as a journal client as long as certain criteria are met. For example, the imuxsock and/or imjournal modules are loaded in the /etc/rsyslog.conf file.

Journal entries can only be viewed using the journalctl utility. To view system journal data, you must either use super user privileges or be a member of the systemd-journal group. To view the entire current journal data file, simply use the journalctl command with no parameters. To screen journal data, use the various filters available with the journalctl utility. Journal files are automatically rotated based on size or time settings in the journal.conf file. Active journal files are moved to archived journal files, their names are modified, and in some cases older archived journal files are deleted. Administrators can also manually clean up archived journal files by employing the journalctl command along with the vacuum options. Manual journal entries are created using the systemd-cat utility, and if the configuration for rsyslog meets certain criteria, the logger command as well.

### Describe the various NTP daemons.

The older ntpd provides NTP client/server services and uses the /etc/ntp.conf as its configuration file. The server lines within the file designate time servers or time server clusters, such as 0.pool.ntp.org, from which to poll for the correct time. You can manually set the system time (software clock) using a server designation in the configuration file via the ntpdate command. You can view time synchronization status using the ntptime utility, and you can view polling activity and statistic through the ntpq -p command.

The newer chronyd provides NTP client server services and uses the /etc/chrony.conf or /etc/chrony/chrony.conf configuration file. The server lines within the file typically designate a single time server, whereas pool lines designate time server clusters, such as 0.ubuntu.pool.ntp.org, from which to poll for the correct time. The rtcsync directive tells chrony to periodically update the hardware time (real-time clock). Utilities that assist in viewing the chrony time sources include the chronyc sources and chronyc sourcestats commands. Additional information can be gleaned from the chronyc tracking utility.

## Review Questions

You can find the answers in the appendix.

- 1. Which of the following are fields within a /etc/passwd file record? (Choose all that apply.)
  - A. User account's username
  - B. Password
  - C. Password change date
  - D. Special flag
  - E. UID

- 2. Which of the following are fields within an /etc/shadow file record? (Choose all that apply.)
  - A. Maximum password age
  - B. Account expiration date
  - C. Password
  - D. Comment
  - E. Default shell

- 3. Which of the following commands will allow you to view the NUhura account's record data in the /etc/passwd file? (Choose all that apply.)
  - A. getent NUhura passwd
  - B. cat /etc/passwd
  - C. passwd NUhura
  - D. grep NUhura /etc/passwd
  - E. getent passwd NUhura

- 4. You create an account using the useradd utility, except for some reason the account's home directory was not created. Which of the following most likely caused this to occur?
  - A. The HOME directive is set to no.
  - B. You did not employ super user privileges.
  - C. The CREATE_HOME directive is not set.
  - D. The INACTIVE directive is set to -1.
  - E. The EXPIRE date is set and it is before today.

- 5. Which of the following commands will allow you to switch temporarily from your account's default group to another group with whom you have membership?
  - A. The usermod command
  - B. The newgrp command
  - C. The groups command
  - D. The groupadd command
  - E. The groupmod command

- 6. Which of the following commands is the best one to add JKirk as a member to a new group called the NCC-1701 group and not remove any of the account's previous group memberships?
  - A. usermod -g NCC-1701 JKirk
  - B. usermod -G NCC-1701 JKirk
  - C. usermod -aG NCC-1701 JKirk
  - D. groupadd NCC-1701
  - E. groupmod NCC-1701 JKirk

- 7. Which of the following could be used to view the members of the NCC-1701 group? (Choose all that apply.)
  - A. groups NCC-1701
  - B. getent group NCC-1701
  - C. getent groups NCC-1701
  - D. grep NCC-1701 /etc/group
  - E. grep NCC-1701 /etc/groups

- 8. What command can you use to check if there is a long list of messages in the email queue? (Choose all that apply.)
  - A. systemctl sendmail status
  - B. sendmail -bp
  - C. sendmail -bq
  - D. mailq
  - E. ls /var/spool

- 9. Your email server receives a message addressed to support. The support address has an alias of wesley on this computer. Assuming the system is properly configured, what account will receive the email message?
  - A. support
  - B. None
  - C. Account in ~/.forward
  - D. root
  - E. wesley

- 10. Which syslog facility keyword represents event messages received from the system job scheduler?
  - A. cron
  - B. user
  - C. kern
  - D. console
  - E. auth

- 11. What syslog severity level has the highest priority ranking in rsyslogd?
  - A. crit
  - B. alert
  - C. emerg
  - D. notice
  - E. err

- 12. What configuration file does rsyslogd use by default?
  - A. rsyslog.conf
  - B. journal.conf
  - C. syslogd.conf
  - D. syslog.d
  - E. syslog.d

- 13. What journalctl option displays the most recent journal entry first?
  - A. -a
  - B. -l
  - C. -r
  - D. -e
  - E. -n

- 14. You want to manually add an entry to the system’s active journal. What command can you use to do this?
  - A. journalctl
  - B. journalctl-cat
  - C. systemd-cat
  - D. systemdd-journal
  - E. journalctl -logger

- 15. Which of the following are clocks or time that can be typically viewed or modified on a Linux system? (Choose all that apply.)
  - A. Hardware clock
  - B. System time
  - C. Software clock
  - D. Atomic clock
  - E. Real-time clock
