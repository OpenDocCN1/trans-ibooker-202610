
said, “GDPR is the most consequential regulatory development in information policy in a generation. The GDPR brings personal data into a complex and protective regulatory regime. That said, the ideas contained within the GDPR are not entirely European, nor new.” Many jurisdictions have introduced similar laws since then.

There have been several high-profile enforcements of GDPR to date. In 2019, Google was fined EUR 50 million, British Airways was fined EUR 183 million, and Marriott was fined GBP 99 million for GDPR violations. GDPR fines have touched not only major corporations but also individuals and governments. A German police officer was fined EUR 1,400 for processing personal data for non-legal purposes in 2019. The Municipality of Bergen in Norway was fined EUR 170,000 for unsafely storing files with login credentials for 35,000 students and employees in a public storage area. There are many such examples and each of them is further raising awareness and actions.

## CYBERSECURITY

In May 2019, the City of Baltimore in the United States was attacked by cybercriminals demanding a ransom of $76,000. They disrupted many of the critical city services. While the city did not pay the ransom, it ended up spending $18 million to address the problems and resolving the issues caused by the attack. Earlier in the year, the Capital One Bank in the United States was breached and personal records of more than 100 million customers were stolen. These are not isolated incidents: every day, every minute there is a cyberattack somewhere in the world. Sometimes it is part of a heinous crime, sometimes it is because somebody is just trying to steal computing resources for blockchain processing.

According to a recent World Economic Forum report published in October 2019, cyberattacks are among the top 10 global risks of highest concern in the next decade, with an estimated price tag of $90 trillion if cybersecurity efforts do not keep pace with technological change. The growth of AI is accentuating the problem even more.

According to the 2019 Cost of Data Breach Report from Ponemon Institute and IBM Security, the global average cost of a data breach has grown by 12% in the last five years to $3.92 million. The United States had the highest cost at $8.19 million, and healthcare had the highest average industry cost of $6.45 million, likely due to their high amount of personal data. The breaches are not only expensive, but they can also cause a significant impact on businesses for a long time. For example, in 2019, the average time taken to identify a breach was 209 days and it took an average of 73 days to respond once they were identified. These numbers are increasing over the years as hackers are also becoming more sophisticated. Fortunately, most of the major breaches are addressed within a much shorter duration.

### Threat Vectors and Vulnerabilities

When considering cybersecurity aspects of an AI initiative, you have to think of it from an end-to-end perspective. AI programs are after all like any other software program that just performs a set of different functions. Hackers use the same set of threat vectors for AI programs, like malware, viruses, email attachments, web pages, popups, instant messages, text messages, and social engineering. However, AI programs have increased vulnerabilities because there are more difficult-to-identify places to inject and bury threat vectors. Given the high usage of NLP interfaces, there is an increased attack surface for threat vectors like instant messages, popups, social engineering, etc. There are many components to this complete ecosystem – the sensors and systems that feed the data, the gateway solutions or APIs that transmit the data, the cloud infrastructure where most of the analytics usually happens, the external sources the cloud systems connect with, and the various enterprise apps that are used. Each of these nodes from where data is generated or where data is processed is vulnerable from a cybersecurity standpoint. Figure 12.1 is a sample representation of the various vulnerability points.

![FIGURE 12.1 Vulnerability Points](img/01839929dba90b77c87d99ac443930ba_215_0.png)

The more sensors, systems, and layers of applications the higher the vulnerability exposure.

The impact of a breach to an organization is potentially staggering, and often unknown until it is too late and it is impacting the organization in many disruptive ways:

- Economic loss due to business disruption, resulting in nonfunctioning goods and services
- Safety and security concerns
- Reputation loss and brand damage
- Increased expenditures due to product maintenance and recalls
- Legal and regulatory fines
- Intellectual property loss

Therefore, several preventive measures need to be put in place:

- Sensors and systems must gather, process, and transmit data using secured methods to the cloud.
- The cloud infrastructure must receive data only from registered and authorized devices and systems.
- Data must be transmitted and stored using encryption and integrity checks to detect tampering.
- Messages sent by a user must be transmitted to an end device using secured methods.
- Business web and mobile applications must ensure role-based access control to ensure only authorized users are permitted to access specific data that is intended to be used by the authorized user.
- The information traveling from a sensor or systems to the cloud and back to the device must be transmitted quickly with minimal delay.

### Cybersecurity Assurance Practices

Cybersecurity assurance is a journey that involves continuous testing and verification processes. Here are six common ones that are best to practice at high frequency:

1. **Open source code scans.** This should be done on the entire technology platform serving the AI project to ensure that there are no publicly known security vulnerabilities present which could lead to exploitation. This will also help avoid any licensing or policy violations.
2. **Static Application Security Testing (SAST).** This is a method of computer program debugging that is done by examining the source code without executing the program. The process provides an understanding of the code structure and can help to ensure that the code adheres to industry security standards. Automated tools are usually used to assist in carrying out static analysis, and the results are then reviewed to filter out false positives and focus on potential weak areas.
3. **Dynamic Application Security Testing (DAST).** This method refers to the testing and evaluation of a program by executing data in real time. The objective is to find errors in a program while it is running, rather than by repeatedly examining the source code offline. By debugging a program in all the scenarios for which it is designed, dynamic analysis eliminates the need to artificially create situations likely to produce errors. This technique can be particularly challenging in AI programs because the output data is not always obvious and therefore any breach might be difficult to detect.
4. **Vulnerability scanning.** There are standard tools that check for known vulnerabilities in network- and host-level components. This is a good practice to follow before any AI program is promoted to a production environment.
5. **Source code assessments.** This is an offline and manual source-code analysis by experienced programmers and architects. When data scientists who are not experienced programmers and may have limited exposure to secure development practices end up writing a lot of programs, this is a good practice to follow. The reviewers can also provide active coaching to the programmers in this way.
6. **Penetration testing.** Penetration testing uncovers vulnerabilities while under normal operating conditions. Ethical hackers are allowed to use all available means to identify and exploit vulnerabilities; they are provided with access to source code and internal documentation to facilitate a more thorough, in-depth analysis.
