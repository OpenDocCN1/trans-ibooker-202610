To thwart bulk email attacks, the concept is to deliberately slow down bulk-delivered emails compared to normal traffic. This may have some impact but typically will just force the spammer to send smaller batches with a longer delay between batches.

More sophisticated tarpitting software is used to monitor ARP requests looking for consecutive requests spaced several seconds apart without any ARP replies. When such requests are discovered, the software creates an ARP reply, using a bogus MAC address, on an unused IP address on the network.

The software then creates a virtual machine that watches for SYN packets for that bogus MAC address and replies with a SNY/ACK that tarpits the connection. This virtual machine might even respond to pings to further the charade.

Sophisticated tarpitting software packages are created to thwart specific worm attacks, but it is an ongoing battle. Sadly, you can’t just enable tarpitting and be fully protected—but this type of software is a great addition to the arsenal.

# Spam

Spammers are generally considered the lowest form of hacker and often perform directory harvest attacks (DHAs), where they simply guess email addresses at a domain and then connect to the email server of that domain. Any addresses that are not rejected are considered valid and can be added to a list of real email addresses the attacker can sell.

Most methods designed to thwart a DHA will result in slower service for legitimate mail. Some of the best solutions involve intelligently monitoring the early stages of mail delivery and denying access for a time to connections that have generated too many invalid recipient errors. Once the spammer’s efforts are blocked, they will probably move on to some other target.

Spammers represent a huge threat today because they have commercial motivation as a startling number of users will actually respond to spam. The spammers don’t have to necessarily breach a network but rather can simply exploit a single user to serve their purposes.

Using social engineering, deceptive emails, clickjacking, or whatever means they can, spammers gather credentials for one of the network user’s email accounts. Even if there is no public webmail server on the network, it isn’t difficult for spammers to figure out server settings for most networks and send email through the mail server as if they were the compromised user.