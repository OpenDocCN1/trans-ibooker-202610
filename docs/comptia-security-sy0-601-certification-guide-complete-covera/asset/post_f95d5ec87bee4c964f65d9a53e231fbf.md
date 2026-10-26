
The following can be used when assessing a person's identity as it needs to be unique to them:

- **Username**: This is the account identity given to the user.
- **Attribute**: This is a unique variable that the user has in their account details, for example, an employee ID.
- **Smart Card**: A credit-card-sized token with a certificate embedded on a chip; it is used in conjunction with a PIN.
- **Certificates**: This is a digital certificate where two keys are generated, a public key and a private key. The private key is used for identity.
- **Token**: This is a digital token that can either be a SAML token used for federation services or a token used by **Open Authentication (OAuth)**.
- **SSH Keys**: These are typically used by an administrator for secure authentication to a remote Linux server, instead of using username and password. First, a key pair (private and public key) is generated. The public key is stored on the server, with the private key remaining on the administrator's desktop.

**Example**: Using a tool such as OpenSSH, the `ssh-keygen -t RSA` command is used to generate an RSA public and private key pair on the administrator's desktop. The next step is to use `ssh-copy-id` to log in to the server and copy the public key across. This is added to the list of authorized keys on the server. While copying, the administrator may be asked to provide their password, whereby the key is generated and copied across. An administrator will use the `ssh root@server` and a user will use `username@server` to test the SSH keys.

- **OpenSSL**: This is a suite that can be used to create and manage Transport Layer Security (TLS) and Secure Sockets Layer (SSL) protocols. In the example below, we are going to create an RSA private key.
- **Create a Private Key**: To create a private key for a Linux server, you would run the following command:

    ```
    openssl genrsa -out rsa.private 4096
    ```

This would create an RSA private key that is 4096-bit.

Account management ranges from account creation on startup to its disablement when someone leaves the company. Fully understanding these concepts is crucial in obtaining the Security+ certification.

## Account Types

Each user in a system needs an account to access the network in a Microsoft Active Directory environment. The user account has a Security Identifier (SID) linked to the account. When I create a user called Ian, they may have a SID of 1-5-1-2345678-345678. When the account is deleted, the SID is deleted too.

For example, a member of the IT team has deleted a user account called Ian. It may have a SID of 1-5-1-2345678-345678, so he quickly creates another account called Ian, but this account cannot access resources as it has a new SID of 1-5-1-2345678-3499999. The first portion from left to right identifies the domain, and then the remainder is a serial number that is never reused.

There are various types of user accounts, and these are heavily tested in the Security+ exam; you must know when you would need each account:

- **User Account**: A user account, also known as a standard user account, has no real access. They cannot install software – they give users limited access to the computer systems. There are two types of user accounts – those that are local to the machine, and those that access a domain. A domain is another name for a large group of users.
- **Guest Account**: A guest account is a legacy account that was designed to give limited access to a single computer without the need to create a user account. It is normally disabled as it is no longer useful, and some administrators see it as a security risk.
- **Sponsored Guest Account**: A sponsored guest account is used for external speakers who may need access to the internet while delivering their presentations.

**Example:** John Smith has been asked by Company A to deliver a presentation to company employees about a new pension plan. While he is delivering the presentation, he wants to show the latest share prices for the stock market. The finance department has asked the IT department to let John Smith access the company network to use the internet. The IT Director decided that the best course of action was to create a sponsored guest account that would allow John to have access to the company guest Wi-Fi.

> **Tip**
> A guest speaker should be allocated a sponsored guest account.

- **Generic Accounts**: Generic accounts are default administrative accounts created by manufacturers for devices ranging from baby alarms to smart ovens and smart TVs. They all have default usernames and passwords. If you surf the web for the device that you have purchased, it is very easy to find the credentials to hack that device. As cybercrime is increasing each day, we should rename the default account name and change the password. Most people purchasing IoT devices are not aware of these accounts and therefore become vulnerable to attack.

**Example**: Baby monitors are getting more sophisticated and come with the ability to see and hear your baby from somewhere else in your home via a web browser. When the police have investigated such instances, they have found out that the baby monitors can be used to film children when they are asleep using the default user account and password. Most parents are unaware that this is possible.

> **Tip**
> If you do not change the default username and password for household devices, known as IoT, it is possible for a cybercriminal to hack into your home. This includes baby monitors, TVs, ovens, and refrigerators.

Let's now look at the different types of authentication.

## Authentication Types

There are various types of authentication, and in this section, we are going to look at these, starting with security tokens and devices. Let's first look at biometric controls, followed by identity management using certificates.

### Security Tokens and Devices

There are different types of tokens that have different time limits. Let's look at the difference between the Time-Based One-Time Password and the HMAC-Based One-Time Password:

- **Time-Based One-Time Password (TOTP)**: A TOTP requires time synchronization because the password needs to be used in a very short period, normally between 30 and 60 seconds. In the following diagram, we can see the TOTP that has come to a phone. It can also come to a device similar to the RSA SecurID token. TOTP could be used when you want to access secure cloud storage or your online bank account:

  ![](img/adfe46a7d545f23d50e4108d31e60de2_95_0.png)

  Use within 30 seconds

  Figure 3.1 – TOTP

- **HMAC-Based One-Time Password (HOTP)**: An HOTP is similar to a TOTP in that a one-time password is issued. The main distinguishing factor is that there is no restriction in terms of time, but you can only use this password once.

Next, we will look at certificate-based authentication.

### Certificate-Based Authentication

Certificate-based authentication is very popular as it provides two-factor authentication, which makes it more secure than single-factor authentication, such as a username and password. We will now look at its various types:

- **Smart Card**: As previously mentioned in this book, a smart card looks like a credit card with a chip on it. The certificate is located on the chip itself and does not leave any trace (footprint) on the computer or laptop being used.
- **Common Access Card (CAC)**: CACs are used by governmental and military personnel as they provide both authentication and identification as it has a picture of the user on it. They are similar to smart cards. On the front side of a CAC is a picture of the user with their service (Army, Navy, or Air Force), and the reverse side shows their blood group and their Geneva Convention Category. To view a CAC card, please go to https://www.cac.mil/Common-Access-Card/.
- **Personal Identity Verification (PIV)**: This is very similar to the CAC but is used by federal agencies rather than the military.

Let's now look at other types of authentication.

### Port-Based Authentication

IEEE 802.1X is a port-based authentication protocol that is used when a device is connected to a switch or when a user authenticates to a wireless access point. Authentication is normally done by certificate.

> **Tip**
> Authentication with a password that has a short lifespan will be a TOTP.
