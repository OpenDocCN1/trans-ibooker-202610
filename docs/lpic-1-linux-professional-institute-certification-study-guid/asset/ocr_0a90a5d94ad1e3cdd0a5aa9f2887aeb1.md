# Listing 7.33: Looking at the /etc/logrotate.d/ files on a CentOS distribution

```
$ ls /etc/logrotate.d/
bootlog glusters libvirtd.qemu psacct syslog
chrony iscsiuiolog numad samba wpa_supplicant
cups libvirtd ppp ssdd yum
$
$ cat /etc/logrotate.d/bootlog
/var/log/boot.log
{
missingok
daily
copytruncate
rotate 7
notifempty
}
$
```

Notice that the /etc/logrotate.d/boot.log contents are similar to the settings for the /var/log/wtmp and /var/log/btmp log files within the /etc/logrotate.conf file. A few of the more common specific logrotate directives for the various log files are shown in Table 7.10.

# TABLE 7.10 The more common logrotate directives for specific log files

| Directive | Description |
| :--- | :--- |
| hourly | Log file is rotated hourly. If this setting is employed, the schedule for the logrotate cron job typically needs modification. |
| daily | Log file is rotated daily. |
| weekly n | Log file is rotated weekly on the n day of the week, where 0 is equal to Sunday, 1 is equal to Monday, 2 is equal to Tuesday, and so on to 6 for Saturday. 7 is a special number that indicates the log file is rotated every 7 days, regardless of the current day of the week. |
| monthly | Log file is rotated the first time logrotate is run within the current month. |
| size n | Rotates log file based on size and not time, where n indicates the file's size that triggers a rotation (n followed by nothing or k assumes kilobytes, M indicates megabytes, and G denotes gigabytes). |
| rotate n | Log files rotated more than n times are either deleted or mailed, depending on other directives. If n equals 0, rotated files are deleted, instead of rotated. |