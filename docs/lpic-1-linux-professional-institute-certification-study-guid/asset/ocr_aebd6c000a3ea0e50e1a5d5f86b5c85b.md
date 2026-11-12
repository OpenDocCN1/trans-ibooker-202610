# Listing 5.12: Finding and displaying a systemd unit file

```
$ systemctl cat ntp.service
# /usr/lib/systemd/system/ntpd.service
[Unit]
Description=Network Time Service
After=syslog.target ntpd.service sntp.service

[Service]
Type=forking
EnvironmentFile=-/etc/sysconfig/ntpd
ExecStart=/usr/sbin/ntpd -u ntp:ntp $OPTIONS
PrivateTmp=true

[Install]
WantedBy=multi-user.target
$
```

Notice in Listing 5.12 that the first displayed line shows the ntpd.service base name and directory location of the unit file. The next several lines are the file's contents. For service unit files, there are three primary configuration sections:

+   - [Unit]
+   - [Service]
+   - [Install]

Within the [Unit] section of the service unit configuration file, there are basic directives. A directive is a setting that modifies a configuration, such as the After setting shown in Listing 5.12. The more commonly used [Unit] section directives are described in Table 5.6.

# TABLE 5.6 Commonly used service unit file [Unit] section directives

| Directive | Description |
| :--- | :--- |
| After | Sets this unit to start after the designated units. |
| Before | Sets this unit to start before the designated units. |
| Description | Describes the unit. |
| Documentation | Sets a list of uniform resource identifiers (URLs) that point to documentation sources. The URLs can be web locations, system files, info pages, and man pages. |
| Conflicts | Sets this unit to *not* start with the designated units. If any of the designated units start, this unit is not started. (Opposite of Requires.) |