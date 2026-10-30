
# Data loss prevention (DLP)

*Data loss prevention (DLP)*, sometimes referred to as *data leakage prevention*, is the set of technologies and practices used to ensure that sensitive data is not lost or accessed by unauthorized parties. DLP technologies can be used to identify and classify sensitive data and apply protections that prevent the data from being “lost” or stolen.

DLP is a foundational component of data protection programs, and it can help restrict the flow of both structured and unstructured data to authorized users, locations, and devices. Simply put, DLP analyzes data storage, identifies sensitive data elements, and prevents users from accidentally or intentionally sending that sensitive data to the wrong recipient.

DLP implementations generally consist of three core stages: discovery and classification, monitoring, and enforcement.

* **Discovery and classification:** The first stage of DLP is discovery and classification. Discovery is the process of finding all instances of data, while classification is the act of categorizing that data based on its sensitivity and value to the organization. While you should have classified your data as part of your information asset inventory, many DLP tools are capable of applying signature-based logic that determines the classification of data. In many cases, your existing classification information can be used to “tune” the DLP to know what you consider sensitive. Examples of classifications might include “PCI data” (or “cardholder data”), “Social Security numbers,” “PHI,” and so on. Comprehensive discovery and proper classification is critical to the effectiveness of the remaining stages and to the success of your overall DLP implementation.

* **Monitoring:** The monitoring stage involves inspecting data as it moves throughout the data lifecycle. During this stage, DLP technologies seek to identify data that is being misused or mishandled. DLP monitoring should happen on workstations, servers, networking devices, and other endpoints, and it should evaluate traffic across all potential exfiltration paths (such as email, internet browsers, messaging applications, etc.). Be mindful that standard DLP implementations cannot monitor encrypted traffic. (See “DLP in transit” in the following list for more details on these limitations.

* **Enforcement:** The final DLP stage, enforcement, is where action is taken to prevent policy violations identified during the monitoring stage. These actions are configured based on the data classification and the potential impact of its loss. For less sensitive data, violations may be automatically logged and/or alerted on, while more sensitive data can actually be blocked from unauthorized exposure or loss. As with any monitoring tool, you must monitor for false negatives and false positives, and constantly tune your DLP implementation to avoid enforcement issues, such as blocking legitimate/authorized data sharing.

When creating a DLP implementation strategy, it's important that organizations consider techniques for protection in every data state (as discussed earlier in this chapter). DLP considerations for each of the data states are discussed here:

* **DLP at rest:** For data at rest, the DLP implementation is located wherever the data is stored. This can include workstations, file servers, or just about any other endpoint with storage. Because data at rest is static, it does not require data loss prevention, in the purest sense. Data in this state is most commonly protected via encryption, access control, and other mechanisms. Although DLP monitoring is still important in this state, it is most effective when combined with other DLP technologies.

* **DLP in transit:** *Network-based DLP* is data loss prevention that involves monitoring outbound network traffic, typically near the perimeter. This DLP implementation monitors traffic over `HTTP`, `FTP`, `SMTP`, and other network protocols. It's important to note here that one limitation of standard DLP implementations is that they cannot effectively monitor encrypted traffic, such as `HTTPS`. If attempting to monitor encrypted network traffic, organizations must integrate encryption and key management technologies into the overall DLP solution; this may significantly increase the complexity of a DLP implementation but is required for certain use-cases.

* **DLP in use:** *Host-based (or endpoint-based) DLP* is data loss prevention that involves installation of a DLP application on a workstation or other endpoint device. This DLP implementation allows monitoring of all data in use on the endpoint and provides insights that network-based DLP systems are not able to provide (such as attempts to transfer data to removable media). DLP systems that monitor data in use can protect against unauthorized copy/paste, screen capture, and other operations that involve active use of sensitive data.

When creating a DLP strategy, organizations must consider how the technology aligns with their existing architecture and processes. DLP controls need to be thoroughly evaluated and applied in a manner that aligns with the organization's overall enterprise architecture to ensure that only the right type of data is blocked from being transmitted. DLP that is incorrectly implemented can lead to false positives that result in legitimate traffic being blocked or false negatives that permit sensitive data to be sent to unauthorized parties.

## Cloud Access Security Broker

With the growth of cloud-based data storage, designated methods for protecting data in the cloud have become essential. A *cloud access security broker* (CASB) is a software application that sits between cloud users and cloud services and applications; CASBs actively monitor all cloud activity and implement centralized controls to enforce security. A CASB may be used to enforce security policies, stop malware, and alert security teams of potential security events — all important contributions to an organization's data security program.

Generally speaking, a CASB aims to serve four primary functions:

* **Visibility:** Provide insight into an organization's cloud usage, including which users use which cloud applications, and which devices are used to connect to cloud resources. CASBs can also help detect, monitor, and secure shadow IT.

* **Data security:** Monitor an organization's data security and help prevent data exfiltration through cloud services. They can also help enforce specific security policies based on the data being used.

* **Threat protection:** Guard against insider threats by providing a comprehensive view of cloud usage.

* **Compliance:** Help organizations demonstrate compliance with regulatory requirements like `GDPR`, `HIPAA`, and `PCI DSS`.

There are three primary types of CASB solutions:

* **Forward proxy:** This type of CASB generally resides on a user's device (e.g., a computer or mobile device) and uses an encrypted man-in-the-middle technique to securely inspect and forward all cloud traffic for the user. This type of CASB requires you to install certificates on every endpoint that needs to be monitored, making it difficult or tedious to deploy in large, distributed organizations.

* **Reverse proxy:** This type of CASB integrates into identity services, such as `Okta`, to force all traffic through the CASB for inline monitoring. This removes the need to individually install certificates on user endpoints but poses compatibility challenges for client-server applications that have hard-coded hostnames.

* **API-based:** This type of CASB allows organizations to protect any user accessing cloud resources from any device, from any location. API-based CASBs monitor data within the cloud itself, rather than on a perimeter-based proxy. There's no need to install anything on user devices, but a potential drawback is that not all cloud applications provide API support for these types of CASBs.

## SUMMARY
