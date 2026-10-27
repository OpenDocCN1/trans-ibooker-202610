
16. ☑ **B.** One of the ways to control tailgating in highly secure environments is to use a mantrap—an area between two interlocking doors in which a person must wait until the first door completely closes before the second door will open to allow access to the building. Solutions such as mantraps or revolving doors are great, but they cost money. A cheap solution in low-security environments is to educate the employees on tailgating and to not open the door if someone is hanging around the entrance.

☒ **A, C, and D** are incorrect. These all require the company to spend money on the solution, and the goal here is to not spend any money.

## Regulation and Standards

17. ☑ **B.** General Data Protection Regulation (GDPR) is a European regulation that is designed to protect private data of individuals. Organizations that process or handle personal data must have security controls in place to ensure unauthorized access to the private data does not occur. GDPR outlines the security requirements and responsibilities of data processors (entities that process personal data) and the data controllers (entities that are responsible for protecting the personal data).

☒ **A, C, and D** are incorrect. PCI DSS has a number of security requirements that must be met by companies that collect credit card data, such as security assessments being performed on a regular basis, network segmentation and security, protection of cardholder information, and implementing strong access control methods to the data. Personally identifiable information (PII) is any information that can uniquely identify a person, such as a Social Security number or driver’s license. ISO 27001 defines how to manage security within the organization, including implementing a security policy, as well as security controls to help with asset management, physical security, and operations security.

18. ☑ **D.** Payment Card Industry Data Security Standard (PCI DSS) has a number of security requirements that must be met by companies that collect credit card data, such as security assessments being performed on a regular basis, network segmentation and security, protection of cardholder information, and implementing strong access control methods to the data.

☒ **A, B, and C** are incorrect. ISO 27002 is a revision to ISO 27001 that defines how to manage security within the organization, including implementing a security policy, as well as security controls to help with asset management, physical security, and operations security. General Data Protection Regulation (GDPR) is a European regulation that is designed to protect private data of individuals. Organizations that process or handle personal data must have security controls in place to ensure unauthorized access to the private data does not occur. GDPR outlines the security requirements and responsibilities of data processors (entities that process personal data) and the data controllers (entities that are responsible for protecting the personal data). The Health Insurance Portability and Accountability Act (HIPAA) was created in 1996 and deals with the privacy of health care records. HIPAA is a U.S. standard whose key area of concern is to protect any individually identifiable health information and to control access to that information.

## Performance-Based Question

19. The following illustration displays the terms matched to their definitions. The Security+ exam may have a similar question, requiring you to drag the box containing the term on the right onto the corresponding description.

| Description | Term |
| --- | --- |
| Informs employees of rules for Internet and e-mail usage | AUP |
| Information that can uniquely identify a person | PII |
| Mechanism used to protect an asset | Security control |
| Informs employees on how to decommission systems and devices | Secure disposal |
| Specifies maximum allowed downtime | SLA |
| Helps detect fraudulent activities performed by an employee | Mandatory vacation |

## Chapter 4: Types of Attacks

### Certification Objectives
- 4.01 Understanding Social Engineering
- 4.02 Identifying Network Attacks
- 4.03 Looking at Password Attacks
- ✓ Two-Minute Drill
- Q&A Self Test

One of the most exciting topics to learn about for your Security+ certification exam is the different types of attacks hackers use to compromise systems and networks. This chapter is important for the Security+ exam, but it's also important for your use in the real world. To understand how to better protect the organization's assets, you need to understand the popular ways that hackers are getting into the systems.

This chapter exposes you to the different types of attacks that are popular today and gives you some history on some older attacks that have been popular in the past. The chapter breaks the attacks into categories—social engineering, network attacks, and password attacks—to help you digest the information better.

### Certification Objective 4.01: Understanding Social Engineering

The first type of attack is known as a social engineering attack. Social engineering involves the hacker trying to trick an employee into compromising security through social contact such as a phone call or e-mail message. This section gives you details on social engineering methods used by hackers.

### Social Engineering Overview

As mentioned, social engineering is when a hacker tries to trick an employee into compromising security through some form of social contact such as calling, e-mailing, texting, or having an in-person conversation with the employee. Most times the hacker will act as if they need help, exploiting the fact that most people will offer help.

A lot of times the hacker is just trying to collect information to help them perform an attack later on, but the hacker may try to trick an employee into divulging usernames and passwords used on the network.

### Popular Social Engineering Attacks

In a number of different scenarios, the hacker tries to extract information from employees or tries to trick them into compromising security. The following discussion represents some of the most popular scenarios that hackers use in social engineering.

### Impersonation

The most popular scenario for social engineering attacks is when the hacker impersonates another employee in the organization. In the following scenarios, the hacker impersonates an employee in the company who needs some help:

- **Hacker impersonates administrator** A very popular example of a social engineering attack is when the hacker calls a user and impersonates the network administrator. In this scenario the hacker, posing as the administrator, tries to trick the user into compromising security by asking the user to do things such as changing their password or giving away account information. The hacker also may ask the user questions about the general setup of the systems.

- **Hacker impersonates user** The hacker calls the network administrator pretending to be a frustrated user. In this scenario the hacker will pretend they do not remember their password or how to get onto the system. An unaware administrator may help the hacker, who is acting as a frustrated user, gain access to the system by resetting a password and guiding them through the process of gaining access.

- **Hacker impersonates management** If the hacker knows the name of personnel on the management team, the hacker may call employees within the company and impersonate management. The hacker will ask the unsuspecting employee to perform actions that will essentially compromise the security of the systems or the environment so that the hacker can gain access later.

> **Exam Watch:** For the Security+ certification exam, know that social engineering attacks involve the hacker trying to trick someone into compromising security through social contact such as a phone call or e-mail message.

### Phishing

Another popular scenario for a social engineering attack is known as a phishing attack. With a phishing attack, the hacker typically e-mails a user and pretends to be a representative from a bank or a company such as eBay. The e-mail typically tells the user that a security incident has occurred and that the user should click the link provided in the e-mail to navigate to the site and check their account status. For example, the e-mail typically looks like it is coming from the security officer for a bank, asking the user to follow the link provided and to log in to the bank site to check their account. The e-mail continues by asking them to report any suspicious transactions to the security officer, with the contact details provided to make the e-mail look authentic.

In this example, the hyperlink that the user is tricked into clicking navigates to a site that the hacker has set up to look like the bank site. The hacker is waiting for the user to try to log on with their account number and password so they can capture it and then store it in a database. Figure 4-1 displays an e-mail I received some time ago trying to trick me into clicking a link that appeared to come from the bank. One of the things you can do to help prevent users from going to a phishing site is to instruct them to always verify the URL the link is pointing to before clicking the link. For example, in most e-mail programs, if you hover over a link, you will see the actual URL pop up as a tooltip, displaying to you the target of the link. Users should also compare the display name of the sender to the actual e-mail address for that display name to see if they match. Also watch for spelling mistakes and bad grammar as another indication that the message could be a social engineering attack.
