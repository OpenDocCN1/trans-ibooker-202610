Another common scenario is one where a company determined certificates to be in their best interests but lacked the on-staff resources to stand it up themselves, and so hired a third party to implement certificates within the network. While this gets certificates rolling, it often leaves a knowledge gap that never gets filled, so you may have a certificate server up and running, but not be at all comfortable modifying or utilizing it.

The broad term for a certificate environment is **public key infrastructure (PKI)**. I call that out specifically because you will probably see PKI listed in documentation or requirements at some point if you haven't already. Your PKI is provided by servers in your network and configuring those servers to issue certificates for you is the purpose of this chapter. The servers that you determine to be your certificate servers are known as **certification authority (CA)** servers, and we will refer to them as CA servers throughout this book.

To get you rolling with certificates in your network, here are the topics that we will cover in this chapter:

+   - Common certificate types
+   - Planning your PKI
+   - Creating a new certificate template
+   - Issuing your new certificates
+   - Creating an auto-enrollment policy
+   - Obtaining a public-authority SSL certificate
+   - Exporting and importing certificates

# Common certificate types

There are a number of different types of certificates that you may find yourself needing to publish. As you will see, when you need a certificate that has a list of particular requirements, you can build a certificate template to whatever specifications you like. So, in a sense, there aren't really certificate *types* at all, but just certificate templates that you scope to contain whatever pieces of information are needed for that certificate to do its job. While this holds true technically, it is generally easier to segment certificates into different groups, making them more distinguishable for the particular job that they are intended to perform.