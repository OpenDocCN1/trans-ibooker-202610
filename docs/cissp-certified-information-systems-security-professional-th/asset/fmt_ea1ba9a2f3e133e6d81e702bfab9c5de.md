
DLP solutions can perform a number of important functions once they have identified data. Discovery is the process of scanning a network to identify data based on pattern matching, signatures, or data labels, and DLP can be used to find unknown stores of sensitive data stored on a network. It can also be used to identify sensitive data moving around a network, like when files are copied from a fileshare to a workstation or attached to an email. Depending on the DLP's configuration, these actions may simply generate an alert, temporarily block the action and ask the user to confirm they intended to send the file, or block the action altogether. The choice of DLP action will depend on the sensitivity of the data in question.

In addition to monitoring data leaving the organization's control, some organizations may implement *ingress* monitoring to identify what data is entering the network. Data that should not be handled or stored may be rejected; for example, an email with a spreadsheet containing credit card information may be rejected by an email server. This prevents the organization from being forced to carry out cleanup and remediation tasks to contain a data breach. Physical ingress and egress monitoring may also be appropriate, such as screening physical media entering or exiting the facility to ensure data is not being transmitted on removable media or the like in contravention of organization policies.

## Log Management

Logs are a vital source of information for a number of security operations activities, chiefly continuous monitoring and IR. Multiple frameworks including ISO 27001 and NIST SP 800-53 define key elements to consider when creating a logging strategy. In addition, NIST SP 800-92 provides a set of critical requirements for securely managing log data, as well as processes for standardizing and analyzing the information collected (`csrc.nist.gov/publications/detail/sp/800-92/final`).

### Define Auditable Events and Thresholds

Defining auditable events and thresholds for logging is a proactive security decision. Determining which events are written into log files and which are not is done before activities occur, and there is a careful balance required. Log too much and the data can become overwhelming, both in terms of storage space and review effort required. Log too little and the organization won't be able to effectively reconstruct details of what happened should an incident occur.

Most platforms offer the ability to set auditable event thresholds based on some event information, while custom-built applications should have documented requirements to support logging. For example, standard informational events like a system status check with no issues noted can be ignored, while changes to vital system configuration files should definitely be logged. Threat intelligence and risks facing the organization should be used to define auditable events—if the organization is facing increased phishing attempts, then increased logging of unusual account activity is justified. Other auditable events to consider include the following:

*   Successful and unsuccessful access attempts like system logins, file or data access, and application access
*   Changes to user permissions, especially escalation like using `sudo` or other admin privileges
*   Changes to or disabling of security tools like DLP
*   Copy or export of sensitive files
*   Sensitive data transactions performed in applications

Logs must contain sufficient detail to reconstruct the actions that occurred on an information system, including who, what, and when. This entails capturing sufficient details about the subjects (users or processes), objects (files, applications, etc.), and action (read, write, execute, delete, etc.). Data points that should be captured to support these requirements typically include the following:

*   User or process IDs
*   Timestamps, ideally in a standardized format like UTC or in a standardize time zone used by the whole organization
*   Device identifier, hostname, IP address, or similar
*   Name of object(s) accessed, like filename or function
*   Policy identifier that triggered the log event, such as a failed login, admin privilege use, or file deletion

### Protect Log Data

NIST SP 800-92 lays out requirements for securely managing and protecting log data, with particular emphasis on preserving the confidentiality and integrity of the data. It includes references to various compliance frameworks that mandate protection of log data, including Payment Card Industry Data Security Standard (PCI DSS), and highlights the critical role of integrity for log data. Information that is logged could become evidence in a future security incident, so the log collection and protection strategy must be designed to provide a defensible basis for authentic data and an unbroken chain of custody. Data contained in the logs may be highly sensitive due to internal confidential information (in the case of system information like IP addresses), or even possible protected information like Personally Identifiable Information (PII) in application logs.

Logs that need to be relied upon in forensics must have an intact chain of custody, so log files are often written to high integrity storage media like write once, read many (WORM) disks, which physically prevent log data from being changed after being written. Storing log files on segregated systems with different access permissions also supports integrity, and SIEM tools can implement this separation of duties. System administrators can tamper with log files on the systems they administer, but if the logs are forwarded to a SIEM tool where they have no access, tampering becomes much more difficult. As log files are written, copied, or forwarded, integrity checks should be performed by hashing the original and storing the hash for later comparison.

Sensitive information may be captured in logs by design, such as system event information, or incidentally, as is often the case with application logs. An application error may contain unexpected data, such as a user's form input that caused the error. An internal data breach is possible if unauthorized users are able to see that information, so protecting logs often requires restricting access to only authorized security personnel. Once again, SIEM or other tools used by SOC personnel can be provisioned separately from systems where logs are generated, thereby avoiding unauthorized access to the data.

Although integrity and confidentiality are crucial for log data, availability is also of concern. Log data increases over time as more events are recorded, so adequate capacity planning is required. This includes ensuring the storage space is sufficient and may also involve determining the length of time audit logs are held before they are overwritten. For local storage on servers, a short period like two weeks may be implemented to conserve space, while on a SIEM tool, the log data may be retained for months, years, or indefinitely. Log data may be archived using offline, cheaper storage methods, which offer slower access at a reduced cost.

## Threat Intelligence

Risk-based security requires a holistic understanding of risk, comprising vulnerabilities that can be exploited by threats. Security practices like patch management focus on identifying and addressing vulnerabilities reactively. Threat intelligence is a proactive activity that allows security practitioners to identify threats and threat actors targeting their organization. For example, threat intelligence may aid in identifying a system compromised by latent malware, which threat actors have access to but have not yet used. In this case, a larger, potentially more serious breach can be prevented by isolating the infected endpoint and removing the malware.

### Threat Feeds
