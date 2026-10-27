
Integrity measurement Integrity measurement in computer security is ensuring that you are aware of any changes that may occur to an environment. To implement the integrity of the system, be sure to use auditing features and hashing technologies to track changes to the system and files.

exam watch For the Security+ exam, know the purpose of sandboxing and also know the order of the different stages to secure deployment of software. The order is development → testing → staging → production.

## CERTIFICATION SUMMARY

In this chapter you learned about the different methods used to implement security on a system. The following list summarizes some key points to remember regarding system security when preparing for the Security+ certification exam:

- Implement a host-based firewall on a system that is connecting to untrusted networks such as wireless or hotel networks.
- Protect systems from malware by installing antivirus and anti-spyware software.
- Most web browsers have phishing filters built in that check a visited site against a known list of phishing sites.
- Install a pop-up blocker to prevent pop-ups from occurring.
- Mobile devices should be secured with a password, and the password should be required after a device has been idle a certain amount of time. Data should be secured by configuring permissions, but also by encrypting the data.

## TWO-MINUTE DRILL

### Host and Application Security Solutions

- Use endpoint security solutions such as malware protection, data loss prevention (DLP) features to prevent data leaks, and host-based firewalls to control communication to a system.
- You can help protect sensitive data on a system by implementing boot integrity tools such as Secure Boot, Measured Boot, and boot attestation tools.
- Tokenization is a solution that involves a token service, creating substitutions for sensitive data and storing a mapping of the substitution with the actual data so that it can be looked up.
- Salting should be used with hashing to help protect passwords stored in a database.

### Implementing Host-Based Firewalls and HIDS

- A host-based firewall should be used on any system that is connecting to an untrusted network.
- Windows and Linux both have firewalls that can be enabled to protect the systems.
- The TCP wrappers feature in Linux allows you to control which systems can connect to the different services.

### Protecting Against Malware

- Ensure that you have virus protection software installed and that the virus definitions are up to date.
- Be sure that you have spam filters installed on the server to prevent spam messages from being sent to your users.
- Ensure that you have anti-spyware and other anti-malware software installed.

### Device Security and Data Security

- Mobile devices should have a password configured so that if they are stolen or lost, others cannot gain access to the information stored on the devices.
- Encrypt data on mobile devices and removable drives so that confidentiality is maintained.
- Trusted Platform Module (TPM) is a secure method of storing keys on a computer chip.
- Use full disk encryption (FDE) to encrypt drives on a mobile device to help protect sensitive data if the device is lost or stolen.
- Use an application whitelisting solution (to control what software is allowed) or an application blacklisting solution (to control what software is disallowed) to limit what software can run on a system.
- Be sure to set up different environments for development, testing, staging, and production in order to increase security of your production environment.
- Create a secure baseline configuration for each type of system on the network and be sure to apply that baseline to the system to ensure the security of the system.

## SELF TEST

The following questions will help you measure your understanding of the material presented in this chapter. As indicated, some questions may have more than one correct answer, so be sure to read all the answer choices carefully.

### Host and Application Security Solutions

1. You are creating custom authentication logic for an application being developed in-house. You have decided to hash the passwords within the database so that if someone gets access to the database, they cannot see the passwords in clear text. What should be used with the hashing to better protect the passwords?
   A. Encryption
   B. Salting
   C. Tokens
   D. Secure Boot

2. You have created an application that involves users using their credit card to purchase products across multiple visits to the site. What protection mechanism is best to use with sensitive data such as credit cards being stored in a database?
   A. Encryption
   B. Salting
   C. Tokenization
   D. Hashing

3. You wish to block users from sending sensitive information in e-mail messages such as credit card numbers or sensitive medical information. What endpoint security feature would you use?
   A. Anti-malware
   B. Host-based firewall
   C. NGFW
   D. DLP

### Implementing Host-Based Firewalls and HIDS

4. Mark is part of the sales team within your organization and spends a lot of time in hotels while on the road. What would you recommend to the administrator with regard to the security of Mark’s laptop?
   - A. Virtualization
   - B. Cloud computing
   - C. TCP wrappers
   - D. Host-based firewall

5. When you enable a host-based firewall, what is typically the default rule applied?
   - A. Allow all traffic.
   - B. Deny all traffic.
   - C. Forward all traffic.
   - D. NAT all traffic.

6. What feature in Linux allows you to configure a list of clients that can access a specific service?
   - A. Virtualization
   - B. Cloud computing
   - C. TCP wrappers
   - D. Host-based firewall

7. How does the HIDS determine that potentially suspicious activity has occurred?
   - A. Log files
   - B. Network traffic
   - C. Packet analysis
   - D. TCP wrappers

### Protecting Against Malware

8. Your manager wants to ensure that a lot of time is not wasted manually patching each system on the network. What would you recommend?
   - A. Virus protection
   - B. WSUS
   - C. Windows Update
   - D. Host-based firewall

## Performance-Based Questions

13. Write the command you would use in Linux to set the permissions on a file called test.txt so that the owner has Read, Write, and Execute permissions, while all others only have Read:

14. Match the following terms to their description by drawing a line from the term on the left to the description on the right.

- COPE
- Geolocation
- Sandboxing
- Geotagging
- Geofencing
- BYOD

Deployment model that encourages users to connect to the corporate network with their personal mobile devices for work purposes

Feature that allows administrators to define GPS coordinates that create a boundary

Identifying the geographic location of an item such as a mobile phone

Store geographical identifying information with media such as photos or video

Creating separate running environments to control communication to an application

Deployment model that involves the user using a company-owned device, but the user can use it for personal usage

15. Place the environments listed on the left in the proper order where applications should be tested.

Order of Deployment for Testing

- Staging
- Production
- Testing
- Development

## SELF TEST ANSWERS

### Host and Application Security Solutions

1. ☑ B. Salting is taking a random value and joining it with the value being hashed, in this case the password, in order to create a hash value that is better protected against password-cracking tools that run dictionary words through hashing in order to crack passwords. ☒ A, C, and D are incorrect. Encryption is not used with hashing; it would be used instead of. Tokens are used with sensitive data in a database such as credit card numbers. Secure Boot is a boot integrity technology that prevents the system from booting from an altered boot state.

2. ☑ C. Tokenization is when sensitive data is substituted with random, nonrelevant data and the mapping of the sensitive data and substituted data is stored in a token service. Applications then send a request to the token service to look up the sensitive data by supplying the substituted data. ☒ A, B, and D are incorrect. Encryption is used to protect sensitive data in a database, but tokenization is common with sensitive data that is used frequently. Salting is when a random value is combined with data and then the combination of the two are run through the hashing algorithm. Hashing is when data is run through a hashing algorithm to create a unique hash value (checksum type value).

3. ☑ D. Data loss prevention (DLP) is a solution that prevents data leaks from occurring, such as someone sending sensitive information in an e-mail message or downloading the data to a USB drive. ☒ A, B, and C are incorrect. Anti-malware is software that prevents the system from virus infection. A host-based firewall is used to limit communication to a system. A next-generation firewall (NGFW) includes firewall features but also intrusion prevention features to stop an attack.

### Implementing Host-Based Firewalls and HIDS

4. ☑ D. A host-based firewall should be used whenever a system is going to be connected to an untrusted network. ☒ A, B, and C are incorrect. These do not represent recommendations to implement regarding the security of Mark’s laptop.

5. ☑ B. Most firewalls have a default rule of deny all traffic once the firewall has been enabled. ☒ A, C, and D are incorrect. These do not represent the default rule. Most firewalls allow you to modify the default rule from deny all to allow all.
