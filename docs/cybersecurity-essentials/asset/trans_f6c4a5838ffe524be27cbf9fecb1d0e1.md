

Popular content management systems such as WordPress are targeted with this sort of application layer attack, often exploiting the search function of the site that appears to be normal requests. The sheer volume of these requests will bring the site down. DDoS mitigation services will help with this, and you could also consider using proxy servers that can provide some sort of rate limiting.

Ultimately, the battle to block IP addresses is rarely won, as these attacks typically come from thousands of IP addresses, depending on the size of the botnet. If a dynamic IP blocking system, such as Fail2ban, is in place some progress might be made. However, a large botnet will probably prevail. IP blocking is a fundamental mitigation concept, but the more distributed the attack the less likely this single tactic will help.

## Tarpitting

Another possible solution to this sort of attack is known as *tarpitting*, sometimes known as a *sticky honeypot*. To mitigate an HTTP attack, the concept here is to initially respond to a connection as normal but with a very small TCP window size. The host replies to a SYN packet with a SYN/ACK, but ignores the sender’s ACK, as shown in Figure 24.10. The sender starts to send data, but the host will never receive it, or the host might just set their TCP window size to zero, effectively stopping incoming traffic.

![](img/2459befd842d6fed299fabe6da3512d0_622_0.png)

**FIGURE 24.10** Tarpitting

Clearly, you can’t do this for all connections, so the challenge is to identify the attackers and only tarpit those connections. This is far better than simply dropping packets because the sender will just send again rather than wait.

To thwart bulk email attacks, the concept is to deliberately slow down bulk-delivered emails compared to normal traffic. This may have some impact but typically will just force the spammer to send smaller batches with a longer delay between batches.

More sophisticated tarpitting software is used to monitor ARP requests looking for consecutive requests spaced several seconds apart without any ARP replies. When such requests are discovered, the software creates an ARP reply, using a bogus MAC address, on an unused IP address on the network.

The software then creates a virtual machine that watches for SYN packets for that bogus MAC address and replies with a SYN/ACK that tarpits the connection. This virtual machine might even respond to pings to further the charade.

Sophisticated tarpitting software packages are created to thwart specific worm attacks, but it is an ongoing battle. Sadly, you can’t just enable tarpitting and be fully protected—but this type of software is a great addition to the arsenal.

## Spam

Spammers are generally considered the lowest form of hacker and often perform directory harvest attacks (DHAs), where they simply guess email addresses at a domain and then connect to the email server of that domain. Any addresses that are not rejected are considered valid and can be added to a list of real email addresses the attacker can sell.

Most methods designed to thwart a DHA will result in slower service for legitimate mail. Some of the best solutions involve intelligently monitoring the early stages of mail delivery and denying access for a time to connections that have generated too many invalid recipient errors. Once the spammer’s efforts are blocked, they will probably move on to some other target.

Spammers represent a huge threat today because they have commercial motivation as a startling number of users will actually respond to spam. The spammers don’t have to necessarily breach a network but rather can simply exploit a single user to serve their purposes.

Using social engineering, deceptive emails, clickjacking, or whatever means they can, spammers gather credentials for one of the network user’s email accounts. Even if there is no public webmail server on the network, it isn’t difficult for spammers to figure out server settings for most networks and send email through the mail server as if they were the compromised user.

## Protecting Against Spam Exploits

Strong passwords and education can provide only so much protection. Once exploited, the spammer can send spam freely through the network and worse yet could even send out false or misleading information that could create liability (or at least embarrassment) for an organization. This type of *repudiation attack* is becoming more and more common. It is far from trivial to limit this sort of attack, but there are some things you can do.

### Require Authentication for the Outgoing Mail Server

Make sure the outgoing mail server requires some sort of authentication. If you don’t allow users to send from outside your network, this is simple. You can constrain sending to a subnet, but most organizations don’t do this. If the outgoing password is different than the incoming mail password, the user base will scream—but it will make it less likely for spammers to achieve this goal. If you require outside users to log in through a VPN to be able to send email, that can help as well provided the VPN is secure.

### Limit Outgoing Email Recipients

You should limit the number of recipients for any outgoing email to a reasonable number. Spammers will want to send large batches of emails as quickly as they can before spam-detection blocks your mail server. If a single email can be sent to only 20 users at a time, this will slow down their progress. If possible, do this per user and accomplish even more while still allowing internal mailing lists and services to function normally.

### Configure Notifications for Email Volumes

You should trigger notification of an administrator when an unusual volume of email is sent by a single user or overall. Generally, when administrators review the logs, they can tell by the email subjects that the message is spam. If you can quarantine bulk email until it can be reviewed, you are in the best position to stop this sort of thing.

### Restrict Webmail Servers

When a public-facing webmail system is used, you should restrict that server more than a normal outgoing mail server because these servers are the favorite targets of spammers. By restricting bulk email sending to traditional email client software rather than webmail systems, you can remove one variable from the equation.

### Pay Close Attention to Email Blacklists

You should monitor email blacklists carefully and respond to complaints immediately. The sooner a rogue account is discovered and disabled, the better. Blacklist-monitoring services that can really help are available. Even though it is too late to prevent a release of spam and subsequent blacklisting by the time the alerts come in, you will at least be in a better position to deal with the issue.

Many email providers and administrators fail to check these lists, assuming they should be above reproach. Blacklisting is a mostly automated activity and what isn’t automated is not governed particularly well.

Spam-weary users may report the mail server and some systems can trigger a blacklisting with very few complaints. Utilize services such as mxtoolbox.com, blacklistmonitoring.com, debouncer.com, and others to assist with blacklisting.

## Other Exploits

There are many other types of attacks and hackers are busy dreaming up new ones every day. Some attacks are just malicious to be malicious, and there is no real commercial gain for the attacker, but others may have commercial motives. With any attack you should look for what possible commercial gain could be available as it may not be immediately obvious.

### Transport Layer Security (TLS) Exploits

As encrypted communication using Transport Layer Security (TLS) is becoming more the rule rather than the exception, it should come as no surprise that more security attacks are focusing on it.

After a secure connection has been negotiated and keys exchanged, there isn’t much that can be exploited, so these attacks focus on this key exchange.

Often, these vulnerabilities exploit weaknesses in both the browser and server that can lead to a connection with weakened encryption. Patching both the client browser and the server SSL/TLS libraries was necessary after an OpenSSL bug was discovered in early 2015 that would allow a man-in-the-middle attack to downgrade connections from strong RSA encryption to an “export-grade” 512-bit RSA encryption.

