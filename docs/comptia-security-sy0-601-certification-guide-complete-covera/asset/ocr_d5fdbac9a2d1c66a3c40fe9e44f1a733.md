+   - **Generic Accounts**: Generic accounts are default administrative accounts created by manufacturers for devices ranging from baby alarms to smart ovens and smart TVs. They all have default usernames and passwords. If you surf the web for the device that you have purchased, it is very easy to find the credentials to hack that device. As cybercrime is increasing each day, we should rename the default account name and change the password. Most people purchasing IoT devices are not aware of these accounts and therefore become vulnerable to attack.

**Example**: Baby monitors are getting more sophisticated and come with the ability to see and hear your baby from somewhere else in your home via a web browser. When the police have investigated such instances, they have found out that the baby monitors can be used to film children when they are asleep using the default user account and password. Most parents are unaware that this is possible.

> **Tip**
> If you do not change the default username and password for household devices, known as IoT, it is possible for a cybercriminal to hack into your home. This includes baby monitors, TVs, ovens, and refrigerators.

Let's now look at the different types of authentication.

# Authentication Types

There are various types of authentication, and in this section, we are going to look at these, starting with security tokens and devices. Let's first look at biometric controls, followed by identity management using certificates.

# Security Tokens and Devices

There are different types of tokens that have different time limits. Let's look at the difference between the Time-Based One-Time Password and the HMAC-Based One-Time Password:

+   - **Time-Based One-Time Password (TOTP)**: A TOTP requires time synchronization because the password needs to be used in a very short period, normally between 30 and 60 seconds. In the following diagram, we can see the TOTP that has come to a phone. It can also come to a device similar to the RSA Secure ID token. TOTP could be used when you want to access secure cloud storage or your online bank account: