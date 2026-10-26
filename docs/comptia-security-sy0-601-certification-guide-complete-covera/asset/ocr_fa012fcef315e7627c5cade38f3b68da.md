**Example:** The following is an LDAP query to find a user called Ian Neil:

```
(&(objectCategory=person)(objectClass=user)(CN=Ian Neil))
```

All transmissions are in clear text and are, therefore, very insecure.

There are different types of email protocols; some are web-based and some use the MAPI client on the desktop. Let's look at the insecure versions:

+   - **SMTP:** This is used to transfer files between different mail servers and is used for outbound emails.
+   - **Post Office Protocol 3 (POP3):** This is an email client that pulls email from the mail server, but when the email is downloaded, it does not retain a copy on the mail server itself. It is not commonly used but is tested in the Security+ exam. There is also a secure version of POP3.
+   - **Internet Message Access Protocol Version 4 (IMAP4):** A mail client that also pulls emails from the mail server, but it has more functionality than POP3 as a copy of the message is retained on the mail server. It can also provide tasks, calendars, and journaling. There is also a secure version of IMAP.

There are two types of **Name Resolution**: hostname resolution, which is the most common, and NETBIOS, which is a legacy name resolution that is very rarely used.

The most common form of name resolution is hostname resolution, where a database of hostnames for IP addresses called a DNS uses a flat file called the hosts file:

+   - **DNS:** This is a hierarchical naming system that takes a hostname and resolves it to an IP address. I don't need to know the actual IP address. If I want to go to the Microsoft website, I know that I need to enter `www.microsoft.com` in my web browser and it will take me there. If I had a user called Ian in a domain called `securityplus.training`, the hostname portion would be Ian, and the **Fully Qualified Domain Name (FQDN)** would be `ian.securityplus.training`. The record types for the DNS database are as follows:
+     a. **A:** IPv4 host
+     b. **AAAA:** IPv6 host
+     c. **CNAME:** Alias
+     d. **MX:** Mail server
+     e. **SRV Records:** Finds services such as a domain controller