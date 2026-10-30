
### Access control
Access should be restricted to individual inboxes and archives. In some cases, a group email inbox is established with multiple recipients. Users with access should be prohibited from granting access to other people. How access control is implemented should be described so that users recognize the balance between legitimate access and some level of privacy. At least, access control deters or prevents unauthorized access.

### Privacy
Users of a corporate email system should generally be accustomed to having no expectation of privacy.

### Email backup and retention policies
Backups and archives are needed for data recovery, legal proceedings, and many audits. Retention rules are important to guide the organization from keeping records that are no longer valuable or needed for legal or regulatory reasons. Excessive record maintenance may lead to security issues of a different kind. Backups can be daily, weekly, or monthly, for example. Retention requirements range from several months of archive to several years. In either case, backup and retention rules must be based on risk, business requirements, and any legal or regulatory guidelines.

### Email Security Solutions
As with all security approaches, a risk-reward (or cost-benefit) analysis must be performed. The cost of implementing and maintaining the security controls should not exceed the value of the information (or the cost of losing it). The security controls should preserve the ability of the business to operate and grow; otherwise, they will be unnecessary, as the business is potentially headed for obsolescence. In terms of email security, a critical consideration to underscore is confidentiality, as email messages may contain valuable and sensitive information to protect.

There are several techniques that organizations can employ to strengthen the protection of their email communications:
* Use digital signatures to combat impersonation attempts
* Subject all incoming email to anti-malware scanning
* Block suspicious attachments and potentially risky filename extensions (such as `.zip` and `.exe`) at the gateway to reduce phishing and similar attacks
* Employ filters to reduce spam and mailbombing problems
* Use encryption to prohibit eavesdropping efforts
* Train users in the importance of and methods for properly recognizing and handling spam or potentially malicious links in email
* Install, run, and update antivirus and endpoint protection

There are several protocols, services, and solutions to augment email infrastructure and provide security without an expensive and complete overhaul of the entire internet-based SMTP infrastructure. Some of these strategies are listed here:
* Secure Multipurpose Internet Mail Extensions (S/MIME)
* MIME Object Security Services (MOSS)
* Privacy Enhanced Mail (PEM)
* DomainKeys Identified Mail (DKIM)
* Pretty Good Privacy (PGP)
* Forced Encryption

`S/MIME` is an email security standard that uses public key encryption and digital signatures to enable authentication and confidentiality for emails. `X.509` digital certificates are used to provide authentication. Public Key Cryptography Standard (`PKCS`) encryption is used to provide privacy. Two types of messages can be formed using `S/MIME`:
* **Signed messages:** To provide integrity, sender authentication, and nonrepudiation of the sender
* **Enveloped messages:** To provide integrity, sender authentication, and confidentiality

`MOSS` is another email security standard that can provide authentication and confidentiality, but also enables integrity and nonrepudiation services for email messages. `MOSS` employs Message Digest 2 (`MD2`) and `MD5` algorithms; Rivest, Shamir, and Adelman (`RSA`) public key; and Data Encryption Standard (`DES`) to provide authentication and encryption services.

`PEM` is an email encryption mechanism that provides authentication, integrity, confidentiality, and nonrepudiation. Like the aforementioned services, `PEM` also uses `RSA`, `DES`, and `X.509`.

`DKIM` is a means to assert that valid mail is sent by an organization through verification of domain name identity. This adds a layer of nonrepudiation and authentication that is particularly helpful in identifying and filtering spoofed addresses in emails. Implementing `DKIM` relies on public keys and digital signing.

`PGP` is a public-private key system that uses a variety of encryption algorithms to encrypt email messages. `PGP` encryption can be used to protect files and other digital assets besides email. The first version of `PGP` used `RSA`, the second version, International Data Encryption Algorithm (`IDEA`), but later versions offered a spectrum of algorithm options. `PGP` encryption can be used to protect files and other digital assets besides email. `PGP` is not a standard, but rather an independently developed product that has wide internet appeal.

Forcing `TLS` for email encryption is another method to provide assurance. A few email service providers such as Apple and Google use `TLS` for `@icloud` and `@gmail`, their respective email services. It is not possible to force `TLS` as a sole option. In fact, `RFC 2487` specifies that a public SMTP server must *not* force `TLS` since its implementation cannot (yet) be made mandatory. However, between email domains for business partners, forcing `TLS` is a viable option.

### Remote Access
Interconnected computing platforms, the strengths of multimedia collaboration, the globalization of business, and the “work from anywhere” movement are some of the forces that have made decentralized workplace arrangements more common. Telecommuting, or working remotely, occurs when a person separated from the main corporate compute and storage platforms accesses them from another location, such as their home, via a laptop. There is also a growing reliance on cloud-hosted resources, where employees on the protected LAN need to remotely access vital infrastructure hosted by a CSP.

When telecommuting or using cloud-based resources, the user might use one of the following access paths:
* Connecting to a network over the internet, with or without a `VPN`
* Connecting to a terminal server system or an access gateway through a thin-client connection

#### Remote Access for Telecommuting
For telecommuting to be productive and successful, employees need to have access to many of the same or comparable resources at a remote location as they would have at the primary office. Access to email, shared storage, and the capability to attend meetings virtually are necessary. Telecommuters use many remote access techniques to establish the required connectivity to the business office network. There are three main types of remote access techniques:
* **Service specific:** If a service such as email via Outlook Web Access (`OWA`) is needed, a service-specific remote access capability allows users to remotely connect to and use it. Service-specific capabilities are not limited to web-enabled versions of on-premises applications. Terminal service access can be limited to a time and attendance application, for example.
* **Remote control:** Common applications for remote control or remote access are help desk or Tier 1 support and employee training functions. Remote control allows an authorized user to gain remote access to another system and take full control as if they were physically in front of the remote system. There are significant security concerns with remote control. Potential threats include brute-force attacks hackers may use to exploit the proprietary Remote Desktop Protocol (`RDP`) that Windows environments use to enable Windows Remote Desktop and Terminal Server.
