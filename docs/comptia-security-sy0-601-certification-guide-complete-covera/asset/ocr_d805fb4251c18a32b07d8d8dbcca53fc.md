# # Syslog/Security Information and Event Management

The systems on a network produce a massive amount of information in log files and most of them will be related to errors or possible attacks. They will require a real-time solution to correlate these events so that the security team can be alerted immediately. Let's look at the role that the SIEM and syslog server play.

**Security Information and Event Management (SIEM)** is regarded as an IT best practice, used by regulated industries to fulfill security and audit compliance regulations, for example, HIPAA, GDPR, SOX, and PCI DSS. SIEM supports IT teams by consolidating event log values through the correlation, aggregation, normalizing standard, and non-standard log formats; it can also filter out false positives.

The only time that a SIEM system will not provide the correct information is when the wrong filters are used or the wrong host is monitored. In these cases, a false positive will be produced.

A **System Logging (Syslog)** protocol server is used to collect data from multiple sources and store them in a single location, such as an event logging database. Legitimate data can be filtered out, thereby reducing the amount of data held. The SIEM can benefit from the filtered data as searching becomes easier. The data between the syslog server and the SIEM system is encrypted.

The different aspects of a SIEM system are shown in the following diagram:

![](bbox=[0.251, 0.575, 0.752, 0.896])