Whenever an email is sent outbound, the internet needs to determine how to deliver that email. Public internet DNS is checked for whichever domain the email is destined for, and the MX record contained within that DNS configuration defines where the email is sent. If you don't have an MX record, or if it disappears for some reason, you will receive zero emails to your domain.

# TXT record

TXT is simply short for text record. TXT records are used within DNS for various purposes, and they can contain just about any kind of information. Sometimes TXT records are placed for actual human reading of some type of info, but more likely you will be asked to create a TXT record at some point as a form of validation.

For example, most websites are protected by SSL certificates, and the process of purchasing an SSL certificate is pretty straightforward. However, you certainly wouldn't want any yahoo with a laptop and internet access to be able to purchase a certificate that can protect a website name that ends in a domain name that your business owns, right? So certification authorities (CAs) (the places where you purchase SSL certificates) will require some sort of validation process before allowing you to purchase a certificate for a domain name. Sometimes that validation process is simply sending an email to an address associated with that domain name, or sometimes you will be asked to create a TXT record with a very exact set of characters in your public DNS records. The implication and validation, of course, is that if you really do own that domain name, you will have access to create DNS records within that domain. When the CA sees your newly created TXT record that contains the data they asked you to input, they know you really do own that domain. An attacker trying to spoof your website and acquire an SSL certificate maliciously would not have such access into your domain's DNS settings.

# SPF Record

There are a few special kinds of TXT records, a common one being Sender Policy Framework (SPF) records. This record pertains to the delivery of emails, but while an MX record identifies which servers email should flow toward, an SPF record identifies which locations email is coming from. SPF records are part of spam and spoofing calculations, used to identify the locations on the internet from which email from your domain is expected to be flowing. Let's take a look at a couple of example SPF records, which will help you to understand how they are formatted, and also assist in describing why they are necessary:

```
v=spf1 [IP address 1] [IP address 2] [include:<domain>] -<enforcement rule>
```