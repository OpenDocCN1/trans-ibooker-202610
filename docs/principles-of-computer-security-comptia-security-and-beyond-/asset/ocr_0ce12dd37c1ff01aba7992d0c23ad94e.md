RIT address will still start with 129.21 in the first two octets.

b. Next in the output you’ll notice a bunch of subdomains that begin with _spf followed by a number. For example, the first one is include:_spf1.rit.edu.

Let’s query for the TXT resource records of that special subdomain in **nslookup**, as if it were an actual domain, by entering the following (the query is still set to txt, so you don’t have to specify that again):
`_spf1.rit.edu`.

Other IP addresses and subnet masks, besides the RIT block, that are authorized to send e-mail on behalf of RIT clients will be shown in the output. Using the ARIN.net Whois lookup tool at https://whois.arin.net/ui/, you can associate these addresses with their corresponding companies. Here are some examples:

+   104.47.0.0/17 Microsoft
+   108.166.43.0/24 Webmail – ORD1c
+   13.111.0.0/18 Xerox Corporation
+   136.147.176.0/20 Salesforce.com, Inc.
+   148.87.89.36 Oracle Corporation

Hostnames consist of letters, digits, and just the dash symbol. No other character is valid. Since domain names can consist of letters, numbers, and any symbol, an underscore was chosen to keep a domain name from being misinterpreted as a hostname. Therefore, _spf1.rit.edu can be used as a domain that contains an SPF policy, without worries that there is an existing host by the name of _spf1.

c. At the end of the SPF output in the rit.edu TXT resource record, you’ll notice ~all.

This means that all other e-mail not meeting any of the listed criteria should be a soft fail. In this situation, the message is not filtered, but the receiving domain’s mail server can use that soft fail in conjunction with other metrics to determine the likelihood that the e-mail is in fact spam. Companies like Microsoft, Google, and Apple use a soft fail since there are third-party vendors that send e-mails for