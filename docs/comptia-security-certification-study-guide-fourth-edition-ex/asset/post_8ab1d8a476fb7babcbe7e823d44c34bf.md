
For example, if you have an intranet server on the network that is designed to deliver web pages to clients, you could enable a host-based firewall on that system and only allow HTTP or HTTPS traffic to reach the system. This is a way to protect that system from malicious traffic, such as if a hacker gets on the network, or maybe malware is on the network. Another example where a host-based firewall should be used is if you are connecting a laptop to an untrusted network such as a wireless network or hotel network. For example, if you have an employee who is traveling and staying in hotels, you want the host-based firewall enabled on the employee’s laptop because the hotel is considered an untrusted network.

### Host-based intrusion detection system (HIDS)
A host-based intrusion detection system looks at logs and events on a system to determine if there is an attack on that system. The intrusion detection system sends out notification that a potential intrusion has occurred, but does not try to correct the problem.

### Host-based intrusion prevention system (HIPS)
A host-based intrusion prevention system is like an HIDS in the sense that it detects potential attacks on the system, but it can take corrective action, such as blocking connections from the IP address that is causing the suspicious traffic. You will learn more about HIDS and HIPS later in the chapter.

exam Watch
For the Security+ exam,
remember the different security solutions
to improve the security of endpoint devices.

### Next-generation firewall (NGFW)
A next-generation firewall is a regular firewall that filters traffic, plus performs deep packet inspection (inspects the payload of the packet), and also includes intrusion prevention features like those found in an IPS.

## Boot Integrity

A common way attackers are gaining access to sensitive company data is by finding a lost laptop or stealing a laptop and then booting off a different operating system located on bootable media such as a bootable USB drive or a DVD drive. Once booted to their own operating system, the hacker can then browse the files on the drive that came with the laptop (assuming it is not encrypted). To help prevent this type of attack, you should use full disk encryption, but you can also use boot integrity features of a system to verify that the boot environment has not changed (booting from a USB/live disc is changing the boot environment). The following are some solutions you can use to ensure the boot integrity:

### Boot Security/Unified Extensible Firmware Interface (UEFI)
Secure Boot is a feature of the UEFI type of BIOS system, which detects if changes to the boot environment have occurred. If the boot environment has changed, the Secure Boot feature will stop the boot process. The boot environment is validated by storing hash values of the boot files and verifying those on startup.

Measured Boot
Measured Boot is similar to Secure Boot in the sense that it verifies the integrity of the boot environment before allowing the system to start. Measured Boot does this by storing hashes of the boot files in a Trusted Platform Module (TPM) chip on the system.

Boot attestation
Boot attestation is when a system attests to a remote system the trustworthiness of the software running on the system. Note that this is a little different from Secure Boot or Measured Boot in the sense that the software is already running and has been verified, and the system is attesting to the verification.

## Database

The Security+ exam expects you to understand security solutions that should be used with sensitive data in the database. This includes tokenization, hashing, and salting.

## Tokenization

Tokenization is when sensitive data is stored with a token service instead of with the application data with which the sensitive data is used. The token service also provides a substitute value for that sensitive data that is nonrelevant. The token service then stores the mapping of the sensitive data and nonrelevant value for future reference so that it can be looked up when needed. For example, the following steps may be used by a web site for online purchases:

- 1. The web site prompts the user for their credit card number.
- 2. When the user enters their credit card number, the web site contacts the token service that is located on a secure network and provides the credit card number to the token service.
- 3. The token service creates a substitute number to be used when making purchases and sends that to the web site to store with customer data in the e-commerce database.
- 4. The token service stores the mapping of the real credit card number and the substitute value in the token service database.
- 5. When the user makes a purchase at a later time, the e-commerce application supplies the substitute value to the token service so that it can retrieve the real credit card number.

The benefit of this tokenization and the token service is that the e-commerce data does not store the credit card number—it is retrieved with the token service. The tokenization system and its database (which does store the credit card number) is located on a restricted, secure network that can only be reached by the web site (and any other authorized applications).

This reduces the risk to a compromise of your credit card number because if the e-commerce site and database are hacked, the credit card number is not there, and the token service database is located on a different network that is restricted.

> exam Watch
> For the Security+ exam, remember that you should protect sensitive data such as credit card numbers with tokenization to facilitate the reusing of the credit cards in a secure manner.

## Hashing

You were introduced to hashing in Chapter 2, but just to review, hashing is when data is run through a hashing algorithm to generate a hash value. One of the uses of hashing is with password storage. Instead of storing a user’s password in the database as plain text, a system may run the password through a hashing algorithm and then store the hash value in the database. This way, if someone hacks the database, they do not see the password in plain text.

So how does authentication work in this scenario if the plain text version of the password is not stored in the database? Simple! When the user logs on to the system, the password they provide is hashed with the same hashing algorithm to generate a hash value. Then the attempted password’s hash value is compared to the hash value in the authentication database, and if they match the system knows the correct password was supplied.

The problem with this technique is in how password-cracking tools work—they take words from a password list file (known as a dictionary file), run the words through the hashing algorithm, and then compare each hash value to the one in the database until they find a match.

## Salting

To prevent the hashed password from being hacked, one technique that can be used is salting, which involves taking a random value, adding it to the plaintext value, and then hashing the combination of the salted value + password and storing that in the database. This salt value is also stored separately so that when a user logs on, their password is joined with the stored salt value and then hashed. If the generated hash value matches the hash value that was stored in the database, the user is authenticated.

The salting protects against cracking the hashed password without knowing the salt value and also ensures that authentication can only occur if the application logic is executing to include the salt value.

> exam Watch
> For the Security+ exam, remember that you should protect passwords stored in a database with hashing combined with salting.

## CERTIFICATION OBJECTIVE 7.02

## Implementing Host-Based Firewalls and HIDS

A big part of creating a secure environment is ensuring that each individual system is securely configured—this includes desktops, servers, mobile devices, and especially laptops used by employees who roam from one network environment to another.

## Host-Based Firewalls

One of the first security applications that should be configured on a system is a host-based firewall, also known as a personal firewall. A host-based firewall is a piece of software that controls inbound and outbound communication to the system. Most networks today have a firewall at the perimeter of the network to control what traffic enters and leaves the network, but you should also configure host-based firewalls on workstations and servers.

I find that most network administrators do not configure host-based firewalls on all workstations and servers within the network, and I can understand why they may not. Configuring a host-based firewall on all systems within the network will make it hard to remotely connect to those systems and to solve issues with them. The host-based firewall should allow appropriate inbound traffic in a corporate environment.

That said, it is important that certain systems have a host-based firewall installed and configured. For example, if you have an employee who travels a lot and connects their company laptop to a hotel or airport network, then you should configure that system with a host-based firewall in order to protect the system from malicious activity on those networks.

> exam Watch
> For the Security+ exam, remember that a system connected to an untrusted network such as a hotel network or any wireless network should be protected by a host-based firewall.

You may also install a host-based firewall on a specific server to ensure that communication to that server is limited to only the type of traffic that is allowed to pass through the firewall. For example, if you have an FTP server on the network and FTP is the only purpose of the server, then you may enable a host-based firewall and open only the FTP ports in order to protect the server against any malicious traffic.
