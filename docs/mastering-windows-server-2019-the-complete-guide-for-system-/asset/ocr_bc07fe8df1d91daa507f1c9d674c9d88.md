Now when I ping intranet, you can see that it resolves to my WEB1 server. And when accessing the web page, I can simply type the word intranet into my address bar inside Internet Explorer in order to launch my page. The website itself is not aware of the name change being made, so I didn't have to make any modifications to the website, only within DNS:

![](bbox=[0.13, 0.18, 0.87, 0.59])

Figure 4.6: Loading WEB1 using the new alias

# Mail Exchanger (MX) record

A third type of DNS record is called a Mail Exchanger (MX) record. In your everyday duties, you will not have to encounter or configure MX records nearly as often as A or CNAME records, but they are important to understand nonetheless. An MX record is all about email services and delivery. Whatever domain name follows the @ in your email address, the DNS servers that are handling that domain name must contain an MX record telling the domain where to point for its mail services. MX records are most commonly used within public DNS, for name resolutions happening over the internet. For companies hosting their own email on local Exchange servers, your public DNS servers will contain an MX record that points at your Exchange environment. For companies hosting their email in a cloud service like Office 365, your public DNS records would need to contain an MX record that directs email traffic toward the cloud provider that is hosting your mailboxes.