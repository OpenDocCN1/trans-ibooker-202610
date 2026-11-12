Combining the facility and severity codes with a short informational message provides enough logging information to troubleshoot most problems in Linux.

# Viewing the History of Linux Logging

Over the years there have been many open source logging projects for Linux systems. The ones that have been the most prominent are as follows:

+   sysklogd: The original syslog application, it includes two programs: the syslogd program to monitor the system and applications for events, and the klogd program to monitor the Linux kernel for events.

+   syslog-ng: Added advanced features, such as message filtering and the ability to send messages to remote hosts.

+   rsyslog: The project claims the “r” stands for “rocket fast.” Speed is the focus of the rsyslog project, and the rsyslog application has quickly become the standard logging package for many Linux distributions.

+   systemd-journal: Part of the systemd application for system startup and initialization (see Chapter 5), many Linux distributions are now using this for logging. It does not follow the syslog protocol but instead uses a completely different way of reporting and storing system and application events.

The following sections dive into the details of the two most popular logging applications: rsyslogd and systemd-journald.

# Logging Basics Using rsyslogd

The rsyslogd application utilizes all of the features of the original syslog protocol, including the configuration format and logging actions. This section walks through how to configure the rsyslogd logging application and where to find the common log files it generates.

# Configuring rsyslogd

The rsyslogd program uses the /etc/rsyslog.conf configuration file and, on some distributions, *.conf files in the /etc/rsyslog.d/ directory to define what events to listen for and how to handle them. The configuration file(s) contains rules that define how the program handles syslog events received from the system, kernel, or applications. The format of an rsyslog rule is

```
facility.priority action
```

The facility entry uses one of the standard syslog protocol facility keywords. The priority entry uses the severity keyword as defined in the syslog protocol, but with a twist. When you define a severity, syslogd will log all events with that severity or higher (lower severity code). Thus, the entry

```
kern.crit
```