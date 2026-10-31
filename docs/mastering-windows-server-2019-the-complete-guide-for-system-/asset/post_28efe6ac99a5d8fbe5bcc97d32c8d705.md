
![](img/d2d47a2ce8c1af61acefc2cdd6d9df70_283_0.png)

Figure 6.27: Setting the encryption bit length to 2048

The only thing left to do for our CSR is to give it a location and filename. Saving this csr as a text file is the normal way to go and serves our purposes well because all we need to do when we request our certificate is open the file and then copy and paste the contents. You have now created your csr file, and we can utilize this file to request the certificate from our public CA.

## Submitting the certificate request

Now, head over to the website for your public certification authority. Again, any of the companies that we mentioned earlier, such as GoDaddy or Verisign, are appropriate for this purpose. Each authority has its own look and feel for its web interface, so I cannot give you the exact steps that need to be taken for this process. Once you have an account and log into the authority's site, you should be able to find an option for purchasing an SSL certificate. For some CAs, the purchasing process and the certificate acquisition process are two different things. For example, you purchase and pay for your new SSL certificate and then you visit a different section of the interface to use the "certificate credit" you just purchased to create a certificate. On the other hand, some CAs walk you through the entire process from start to finish. In any event, once that certificate has been purchased, there will be a process for requesting and deploying that certificate.

Once you have found the interface used for generating the new certificate, you will generally be asked for four pieces of information:

- 1. **Validity period** – How long should this SSL certificate last? It used to be common that companies would purchase SSL certificates with 1-year, 2-year, or even 3-year validity periods, meaning that the certificate was "good" for that amount of time. Recently, Apple has flexed its muscles in this space and decided that the Safari web browser will only trust certificates that have a validity period of 1 year, more or less. Rather than push back, the industry said "sure, why not?" and so moving forward, SSL certificates will only be valid for 1 year at a time.

- 2. **Webserver platform** – What type of webserver are you running? The answer to this question impacts what type of file is available to download at the end of this process. When working with Windows Server, your answer will be "IIS" but possible others include Apache, Tomcat, NGINX, and many more Linux-based webserver selections.

- 3. **Domain ownership validation** – This one is interesting, and if you haven't been through this process before, you likely haven't thought about why it is necessary. What's to stop hacker Joe sitting in an internet café halfway across the world, or the 12-year-old playing video games in his basement, or even your next-door neighbor from creating their own CA login and purchasing an SSL certificate that has *your* company name and website on it? The validation of domain ownership is a process to prove that you really own contoso.com (or whatever your domain is) so that only you are allowed to purchase valid SSL certificates that protect traffic for your domain name. The most common way to validate domain ownership is to select from a list of pre-defined email addresses inside the CA and have it send some flavor of validation email to that email address. For example, you might have a selection of email addresses like the following:

```
admin@contoso.com
administrator@contoso.com
webmaster@contoso.com
hostmaster@contoso.com
```

- 4. All you need to do is verify that you can receive an email to one of those email addresses, and by selecting this option you will receive an email to that address that usually contains a link and a code. Simply copy the code and paste it into the link, and this is an easy way to validate domain ownership because as the owner of that domain, only YOU would have access to those email addresses. A second form of ownership, if email doesn't pan out, is that the CA may give you a special DNS record that can be put into place in your domain's public DNS. You input the record, the CA sees that record show up on the internet, and they validate you are the domain owner. This process takes much longer to accomplish than email verification, so it is rarely used.

- 5. **CSR** – The last (and most important) thing you will be asked to provide is the text from inside that CSR file we created a few minutes ago. There will be either an upload function or simply an input box, and all you need to do is copy/paste the entire contents of the CSR into that box. If you open the text file we saved earlier, you will see a big lump of nonsense:

![](img/d2d47a2ce8c1af61acefc2cdd6d9df70_285_0.png)

Figure 6.28: The text inside a CSR file

This mess of data contains information about your certificate request and is exactly what the CA needs in order to create your new SSL certificate so that it knows how to interact with your webserver's private key. Only the server that generated the CSR will be able to accept and properly utilize the SSL certificate that is based on this CSR.

## Downloading and installing your certificate

Now you sit back and wait. Depending on which authority you are using and on how often your company purchases certificates from this authority, your certificate might be available for download almost immediately, or it could take a few hours before that certificate shows up in the available downloads list. The reason for this is that many of the CAs utilize human approval for new certificates, and you are literally waiting for someone to put eyes on the certificate request and your information to make sure you really work for the company and that you really own this domain name. Remember, the real benefit to a public SSL cert is that the CA is guaranteeing that the user of this certificate is the real deal, so they want to make sure they aren't issuing a certificate for portal.contoso.com to someone in the Fabrikam organization by mistake.

Once you are able to download the certificate from the CA website, go ahead and copy it over to the webserver from which we generated the CSR. It is critically important that you install this new certificate onto the **same server**. If you were to install this new certificate onto a different webserver, one that did not generate the CSR this certificate was built from, that certificate would import successfully, but would not be able to function. Once again, this is because the private key that the certificate is planning to interact with would not be present on a different server.

Back inside the IIS management console, we can now use the next action in that list on the right, called **Complete Certificate Request...**. This launches a short little wizard in which you point at the newly downloaded certificate file, and the wizard then imports it into our server. Now that the certificate resides on the server, it is ready to be used by your website.

There is one additional item that I always check after installing or importing an SSL certificate. You can now see your new certificate listed inside IIS, and if you double-click on your new certificate, you will see the **Properties** page for the certificate. On the **General** tab of these properties, take a look near the bottom. Your certificate should display a little key icon and the **You have a private key that corresponds to this certificate** text. If you can see this message, your import was successful and the new certificate file matched up with the CSR perfectly. The server and certificate now share that critical private key information, and the SSL certificate will be able to work properly to protect our website. If you do not see this message, something went wrong in the process to request and download our certificate. If you do not see the message here, you need to start over by generating a new CSR, because the certificate file that you got back must not have been keyed appropriately against that CSR, or something along those lines. Without the *You have a private key that corresponds to this certificate* text at the bottom of this screen, your certificate will *not* validate traffic properly.
