
There are different types of messages or entries with syslog, with each message type using a different log level. The following table shows the common log levels and messages with syslog:

| Log Level | Message |
|---|---|
| 0 | Emergency (System unstable) |
| 1 | Alert |
| 2 | Critical |
| 3 | Error |
| 4 | Warning |
| 5 | Notification |
| 6 | Informational |
| 7 | Debugging |

The data is sent from the system or device to the syslog server over UDP port 514 for unencrypted communication, or TCP port 6514 for encrypted communication with TLS. Once the information is logged at the syslog server, you can centrally review the logs of all systems and devices on the syslog server and even filter entries out looking for specific events.

The high-level steps to configure syslog in your environment are as follows:

1. Install a syslog server. There are some free ones, such as Kiwi Syslog Server, you can download from the Internet to test syslog capabilities.
2. Configure the system, application, or network device to forward events to the syslog server.
3. Review the logs on the syslog server.

Let’s assume you installed the Kiwi Syslog Server on a system with an IP address of 10.0.0.5. If you wanted to configure your Cisco router to forward all error-level events (and lower) to the syslog server, you would configure the router with the following commands:

```
R1>enable
R1#config term
R1(config)#logging host 10.0.0.5
R1(config)#logging trap 3
```

You can then control what the source IP address of the log messages is with each message by setting the source interface on the Cisco device:

```
R1(config)#logging source-interface Loopback0
```

- **Sentiment analysis**: A monitoring system may have the capabilities to perform sentiment analysis, which involves reviewing the information to determine if that information is of a positive nature, a negative nature, or is neutral.
- **Security monitoring**: The monitoring tool should perform monitoring of security events, which typically comes from security logs on different systems.
- **Log aggregation**: As mentioned earlier, the tool should be able to use different types of logs from different systems and put them together to do a complete analysis.
- **Log collectors**: The monitoring tool should have log collector components that allow it to collect the logs from different systems and devices on the network.

Various tools are available that fall into the category of SIEM tools, such as Splunk, Tripwire, EventLog Analyzer, and SolarWinds. The following are the high-level steps for installing EventLog Analyzer, which you can download as a 30-day trial from www.manageengine.com/products/eventlog/download.html?edi. Like many of the SIEM tools, EventLog Analyzer has prebuilt reports for various regulatory mandates such as HIPAA and ISO 27001.

1. Download and run the installer.
2. Accept all defaults. At the end of the installation, choose the option to start EventLog Analyzer in console mode.
3. Once the server starts, you are taken to the web interface for EventLog Analyzer, where you need to log on with the username of **admin** and the password of **admin**.
4. Once you have logged on, you are placed on the dashboard, where you can see charts of different types of events from your server (see Figure 19-4).

**Figure 19-4** — EventLog Analyzer SIEM tool

![](img/3e3af33e2c753e920c09ecd40b5a457a_789_0.png)

You can view built-in reports by clicking the Reports link at the top of the admin page. There are a number of reports that display information on security events, threats, and firewall auditing, to name a few. Remember that these reports are displaying data from logs across all of your systems and devices; to add a device to EventLog Analyzer, click the green Add button in the top-right corner.

## Working with SOAR

While talking about enterprise solutions that help us monitor for security events, we should also discuss the concept of *Security Orchestration, Automation, and Response*, or SOAR for short. SOAR is more than a monitoring tool; it is more like a security management tool. With SOAR, you use software tools that help you streamline and automate your security operations with three main areas of focus—threat and vulnerability management, security incident response, and security operations automation.

SOAR can be used by companies to collect security event data from different sources, detect a security threat, and then automate a response to the threat to take some form of corrective action.

## CERTIFICATION OBJECTIVE 19.03: Implementing Logging and Auditing

Now that you understand some of the tools you can use to monitor a system, let’s talk about auditing and logging. As a security professional, you need to make sure you assess the level of auditing and logging that has been enabled on the systems and the applications. The following is a quick definition of the difference between auditing and logging:

- **Auditing**: Typically refers to actions that you wish to monitor on a system or application for security purposes. For example, you may wish to audit the management of user accounts on a system or the deletion of a customer record in an application.
- **Logging**: Typically refers to logging all activity that occurs in an application or on a system. For example, you can log all requests to a web site and review the logs later.

### Understanding Auditing

When working as a security professional in any environment, ensure that auditing has been enabled on all critical systems and devices and also determine to what level of auditing has been configured. It is critical that someone is trained on how to review the audit logs and knows how to archive them to a secure location in case you need to go back to them.

There are four major steps to configuring auditing:

1. Decide which events to audit.
2. Decide whether you care about the success or failure of each event.
3. Configure auditing.
4. Review the audit logs on a regular basis.

### Configuring Auditing

The first step to configuring auditing is to define your audit policy. When defining your audit policy, enable auditing for specific events within the operating system. An event is something that happens within the system and is usually invoked by a user. For example, logging on or logging off the network is an event. Accessing a folder is also an event. So you need to determine which events you wish to be notified of—and believe me, you need to be extremely picky as to which events you enable! The important thing to remember is that the more events you audit, the more information that is collected—and you don’t want to collect unnecessary information, because it might obscure the important audited data.

In Windows, you can implement auditing through the local security policies of a single system, or you can deploy the auditing settings across multiple systems by using group policies in Active Directory. To configure auditing in Windows, choose from one of the following tools to manage the audit policy:

- **Start | Administrative Tools | Local Security Policies** — Implement auditing on a single stand-alone system.
- **Start | Administrative Tools | Domain Controller Security Policy** — Implement auditing on all the domain controllers on the network.
- **Start | Administrative Tools | Domain Security Policy** — Implement auditing on all systems in the entire Active Directory domain.

Once you launch the correct administrative tool, you can enable auditing for a number of events (see Figure 19-5), and when you enable auditing, you have to decide whether you care about the success or the failure of such events. For example, do you care whether someone successfully logs on to the network or fails to log on to the network?

**Figure 19-5** — Configuring an audit policy on a Windows Server

![](img/3e3af33e2c753e920c09ecd40b5a457a_792_0.png)

Table 19-1 lists the various auditing events in Windows whose success or failure you can enable.

### Reviewing Audit Logs

Once you have configured auditing, you will look to Windows Event Viewer to monitor the security-related events for the auditing you have enabled. The following is a listing of some of the logs you can review in Windows Event Viewer:

- **Application log** – Stores errors and information about applications running on Windows
- **System log** – Stores errors, warnings, and information about operating system components, such as when a service fails to start
- **Security log** – Stores the auditing information and other security-related events
- **DNS log** – Stores errors and information about DNS on a domain controller
- **Hardware Events log** – Stores information related to hardware components on the system
- **Windows PowerShell log** – Stores information and errors related to the PowerShell environment on the system

Once you have enabled auditing, you monitor the Security log in Windows Event Viewer because that is where all audited data appears. Figure 19-6 shows Windows Event Viewer in Windows Server. In Event Viewer, you can review the various events, save the contents
