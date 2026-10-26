
- **Data Inputs**: The SIEM system collects a massive amount of data from various sources. Some examples are the following:
- **User Behavior Analysis (UBA)**: This is based on the interaction of a user that focuses on their identity and the data that they would normally access on a normal day. It tracks the devices that the user normally uses and the servers that they normally visit. If you look at the behavior of one person, you may not identify attacks, but if you apply UBA to a whole company then you start to identify potential attackers as they deviate from normal patterns.
- **Sentiment Analysis**: Consumer sentiment can be measured by looking at Amazon, Twitter, LinkedIn, and Facebook where people submit reviews on products by inserting emojis and pressing the like or share buttons. Marketing companies can analyze this to get a good idea of consumers' feelings, according to which they can then tailor their marketing to their target market. Similarly, in cybersecurity, we use artificial intelligence and machine learning to identify attacks. With cybersecurity sentiment analysis, we monitor articles on social media, and look at the text so that we can analyze the sentiment behind the articles. If this is analyzed over a period of time, we can get a good grasp of users' attitudes to different aspects of cybersecurity.
- **Security Monitoring**: This is a real-time protection and event monitoring system that correlates the security events from multiple resources, identifies a breach, and helps the security team to prevent the breach.

| Intrusion detection systems | Desktop events |
| Firewall logs | Server events |
| Network packets | Antivirus events |
| Application servers | Database logs |
| Switches logs | Router logs |

In the next section, we will look at **Security Orchestration, Automation, and Response (SOAR)** in detail.

## Threat Hunting

Threat hunting is a dynamic process of seeking out cybersecurity threats inside your network from attackers and malware threats. According to the Security Intelligence website, an average cybercriminal can spend 191 days inside your network before being discovered. Please look at this article at the following link for more information:

https://securityintelligence.com/a-beginners-guide-to-threat-hunting/.

When you are going to carry out threat hunting, you first need to gain as much information from open-source intelligence, feeds, advisories, and bulletins as needed to determine what you are looking for. You must also know your environment and how the applications and processes interact with each other so that you can identify what is and is not a threat. You must adopt the mindset of being an attacker and look for evidence of cyber-attacks, but also look for any vulnerabilities from your research so that they can be secured.

Let's now look at the process of threat hunting so that we can understand it better. We will look at intelligence fusion, threat feeds, advisories and bulletins.

- **Intelligence Fusion:** Open-source intelligence is available on the internet from various places such as vendor blogs, researchers, and websites such as *security week*. Please visit this URL for further details: `https://www.securityweek.com/virus-threats`. Companies such as *Crowdstrike* produce open-source intelligence reports. Please refer to their 2020 report, which is available here: `https://go.crowdstrike.com/crowdstrike-2020-overwatch-threat-hunting-report-thank-you.html`. From these resources, you will have a better idea of the current threats and what you will be looking for.
- **Threat Feeds:** Open-source threat feeds provide the latest information. Please look at the best resources for threat feeds listed on the *logz* website at the following URL: `https://logz.io/blog/open-source-threat-intelligence-feeds/`.
- **Advisories and Bulletins:** Advisories and security bulletins provide good advice on how to keep your company safe. The advisories tend to be government-funded agencies. Bulletins tend to be released by vendors or private companies. Please look at the bulletin from the OPC foundation at the following link: `https://opcfoundation.org/SecurityBulletins/OPC%20Foundation%20Security%20Bulletin%20CVE-2017-12070.pdf`.

**Example:** Let's look at two advisories, one in the US and the other in the UK.
The *Cyber Security and Infrastructure Agency*, which is part of *US Government Homeland Security*, informs the public about threats. This helps you to protect your company. Please refer to *Alert (AA20-245A)* at https://us-cert.cisa.gov/ncas/alerts/aa20-245a. In the UK, the *National Cyber Security Centre* lists advisories at the following URL: https://www.ncsc.gov.uk/section/keep-up-to-date/reports-advisories.

- **Maneuver:** To use a martial example, in a war, one side would out-maneuver the other side by disguising their attacks and then acting rapidly to gain an advantage over an opponent. A cybersecurity maneuver, then, refers to a company's efforts to defend itself by disguising its systems, thereby making it difficult for an attacker to successfully infiltrate. In threat hunting, performing a defensive maneuver means knowing the attack methods that a potential attacker will use so that you can intercept them and disrupt their attack when they least expect it.

## Review Questions

Now, it's time to check your knowledge. Answer the questions, and then check your answers, which can be found in the *Solutions* section at the end of the book:

- 1. Which pen tester would be given source code?
- 2. Why would a shared account pose a problem to monitoring?
- 3. Which pen tester would be given no access before the test but, at the last minute, is given a diagram of the desktops?
- 4. What needs to be established prior to a pen test commencing?
- 5. While carrying out an unannounced pen test, how does the tester know if the internal security team is on to him?
- 6. What is the scope of rules of engagement?
- 7. If the pen test has been announced to the IT team, what information should they give regarding the test before the test start?
- 8. What is the main difference between a credentialed and a non-credentialed vulnerability scan?
- 9. At what phase of a pen test does the tester return the systems to the original state or inform the IT team of vulnerabilities that need patching?
- 10. What is OSINT? Is it legal?
- 11. What is the purpose of the red team?
- 12. What is the purpose of the blue team?
- 13. What is the purpose of the white team?
- 14. What is the purpose of the purple team?
- 15. When evaluating CVSS scores, which vulnerabilities should you deal with first?
- 16. Describe a false positive.
- 17. What is a true positive?
- 18. What is the difference between intrusive and non-intrusive scans?
- 19. What is regression testing and who will carry it out?
- 20. When would dynamic analysis be carried out?
- 21. What is a syslog server and what purpose does it serve?
- 22. Why does a SIEM server rely on synchronized time clocks between all servers and devices that it collects data from?
- 23. What is the purpose of threat hunting?

## Introduction to Protocols

**Protocol** refers to the rules required by different applications for the exchange of data, where the application can perform actions such as running commands on remote systems, sending and receiving emails, and downloading files from the internet. Each application has a special port number that it uses for communication. You can think of ports as being TV channels: if we want to watch sport, we go to the sports channels; if we want to watch the news, we go to the news channel. Applications are the same (if we want to send an email, we use an email application), and they all have a distinct port number for each communication mode.

There are two types of ports: **Transmission Control Protocol (TCP)** and **User Datagram Protocol (UDP)**. The main difference between the two is that TCP is connection-oriented as it uses a three-way handshake, and UDP is faster but less reliable as it is connectionless. The following diagram shows the three-way handshake:

![](img/adfe46a7d545f23d50e4108d31e60de2_182_0.png)

Figure 6.1 – Three-way handshake

In a three-way handshake, the first packet that is sent is called an **SYN** packet, where the sending host informs the receiving host of the number of its next packet. The receiving host sends an **SYN/ACK** packet, where it says what its next packet is. The **ACK** packet acknowledges both kinds of packets, and then the data is sent. The data is sent in chunks, and when it is received, an acknowledgment is sent that tells the sending host to send more packets. Once all of the data is sent, a three-way handshake confirms that all of the data is intact and the session closes.

In a UDP session, the application is responsible for ensuring that everything is received, and because a three-way handshake is not used, the connection is faster but less reliable. You would use UDP for streaming video and gaming, where speed is paramount.
