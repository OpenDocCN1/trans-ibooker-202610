
Now that we have created this new host record, we should immediately be able to start resolving this name inside our domain network. Moving back to the client machine from which I was trying to ping RA1 earlier, I'll try the same command again, and this time it does resolve and reply successfully:

![](img/d2d47a2ce8c1af61acefc2cdd6d9df70_166_0.png)

Figure 4.4: Successfully pinging RA1

## Alias record – CNAME

Another useful type of DNS record is CNAME, which more commonly these days is called an alias record. This is a record that you can create that takes a name and points it at another name. It sounds a little silly at first glance, because, in the end, you are still going to have to resolve your final name to an IP address by using a host record in order to get the traffic where it needs to go, but the purposes of an alias record can be vast. A good example to portray the usefulness of an alias record is when you are running a web server that is serving up websites within your network. Rather than force all of your users to remember a URL like http://web1.contoso.local in order to access a website, we could create an alias record called intranet, and point it at WEB1. This way, the more generalized intranet record can always be utilized by the client computers, which is a much friendlier name for your users to remember.

In addition to creating a happier user experience with this new DNS record, you have, at the same time, created some additional administrative flexibility because you can easily change the server components that are running beneath that record, without having to adjust any settings on the client machines or retrain employees on how to access the page. Need to replace a web server? No problem, just prep the new server alongside the old one and then point the alias record at the new server. Need to add another web server? That's easy too, as we can create multiple alias records, all with the same intranet name, and point them at the different web servers that are in play within the environment. This creates a very simple form of load balancing, as DNS will start to round-robin the traffic among the different web servers, based on that intranet CNAME record.

In fact, rather than continue to talk about this, let's give it a try. I have a website running on exactly that URL in my environment, but currently I can only access it by typing in http://web1.contoso.local. Inside DNS, I am going to create an alias record that redirects intranet to WEB1:

![](img/d2d47a2ce8c1af61acefc2cdd6d9df70_167_0.png)

Figure 4.5: Creating a new alias record

Now when I ping intranet, you can see that it resolves to my WEB1 server. And when accessing the web page, I can simply type the word intranet into my address bar inside Internet Explorer in order to launch my page. The website itself is not aware of the name change being made, so I didn't have to make any modifications to the website, only within DNS:

![](img/d2d47a2ce8c1af61acefc2cdd6d9df70_168_0.png)

Figure 4.6: Loading WEB1 using the new alias

## Mail Exchanger (MX) record

A third type of DNS record is called a Mail Exchanger (MX) record. In your everyday duties, you will not have to encounter or configure MX records nearly as often as A or CNAME records, but they are important to understand nonetheless. An MX record is all about email services and delivery. Whatever domain name follows the @ in your email address, the DNS servers that are handling that domain name must contain an MX record telling the domain where to point for its mail services. MX records are most commonly used within public DNS, for name resolutions happening over the internet. For companies hosting their own email on local Exchange servers, your public DNS servers will contain an MX record that points at your Exchange environment. For companies hosting their email in a cloud service like Office 365, your public DNS records would need to contain an MX record that directs email traffic toward the cloud provider that is hosting your mailboxes.

Whenever an email is sent outbound, the internet needs to determine how to deliver that email. Public internet DNS is checked for whichever domain the email is destined for, and the MX record contained within that DNS configuration defines where the email is sent. If you don't have an MX record, or if it disappears for some reason, you will receive zero emails to your domain.

## TXT record

TXT is simply short for text record. TXT records are used within DNS for various purposes, and they can contain just about any kind of information. Sometimes TXT records are placed for actual human reading of some type of info, but more likely you will be asked to create a TXT record at some point as a form of validation.

For example, most websites are protected by SSL certificates, and the process of purchasing an SSL certificate is pretty straightforward. However, you certainly wouldn't want any yahoo with a laptop and internet access to be able to purchase a certificate that can protect a website name that ends in a domain name that your business owns, right? So certification authorities (CAs) (the places where you purchase SSL certificates) will require some sort of validation process before allowing you to purchase a certificate for a domain name. Sometimes that validation process is simply sending an email to an address associated with that domain name, or sometimes you will be asked to create a TXT record with a very exact set of characters in your public DNS records. The implication and validation, of course, is that if you really do own that domain name, you will have access to create DNS records within that domain. When the CA sees your newly created TXT record that contains the data they asked you to input, they know you really do own that domain. An attacker trying to spoof your website and acquire an SSL certificate maliciously would not have such access into your domain's DNS settings.

## SPF Record

There are a few special kinds of TXT records, a common one being Sender Policy Framework (SPF) records. This record pertains to the delivery of emails, but while an MX record identifies which servers email should flow toward, an SPF record identifies which locations email is coming from. SPF records are part of spam and spoofing calculations, used to identify the locations on the internet from which email from your domain is expected to be flowing. Let's take a look at a couple of example SPF records, which will help you to understand how they are formatted, and also assist in describing why they are necessary:

```
v=spf1 [IP address 1] [IP address 2] [include:<domain>] -<enforcement rule>
```

The preceding is standard formatting for an SPF TXT record. For simple email systems where all email flows from one public IP address on the internet, or from one hosted email system, your record will be nice and short. A great example is email coming from Office 365. Everyone who hosts email in Office 365 should have an SPF record with the following information inside:

```
v=spf1 include:spf.protection.outlook.com -all
```

SPF record information is used by receiving email servers to validate the mail coming inbound. That mail server will receive the email, discover which domain it came from, and reach back out over the internet to that domain's DNS servers to check for the existence of an SPF record. If it doesn't find such a record, your email has a much higher chance of being flagged as spam. If it does find an SPF record, the mail server then checks the entries in that record, which identifies the safe places that we expect mail to be flowing from, and confirms that the email did, in fact, originate from one of those places. In our preceding Office 365 SPF record, we included the domain spf.protection.outlook.com. All email that flows from Office 365 servers will match this origination, so this simple record is the only thing needed to be in place for safe delivery of email from your Office 365 tenant.

Now pretend that your SPF record is already in place and working with your hosted email, but that you set up an on-premise SMTP relay server so that your copiers on the network have the ability to scan to email. This is a common request by businesses, and something that I find in the wild on a very regular basis. The nature of your SMTP server doesn't really matter; there are various ways to accomplish that. What you'll usually find, though, is that when you send email from your copier, that email may start getting flagged as spam. Sometimes it lands in the recipient's junk mail folder, sometimes it is captured and not delivered to them at all. Why is this? If you have an onsite SMTP relay where the email is coming from, remember the copier just hands email over to the SMTP relay, and that relay is the guy who is shuttling the email over the internet. That relayed email is not coming from spf.protection.outlook.com, but rather email sent from your on-premise server is coming from your ISP outbound public IP address. Your SPF record needs to be adjusted to include this additional safe sender, so that recipients of your email will know that emails coming from your building are legit. Let's build out an SPF record that includes a single IP address:

```
v=spf1 ip4:8.8.8.8 include:spf.protection.outlook.com -all
```
