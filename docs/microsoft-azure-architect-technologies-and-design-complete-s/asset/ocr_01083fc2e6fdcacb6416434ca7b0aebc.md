Application Gateway is a network security appliance that, in addition to load balancing, managing session affinity, and URL rerouting capabilities, provides a web application firewall (WAF).

Web applications are one of the most vulnerable endpoints for exploitation. The reason is simple: they are globally available to anyone and anything that also has connectivity to the internet. Keeping up with known vulnerabilities and patches to prevent them can be a significant undertaking depending on the size of your application or number of applications. There are vulnerabilities not only from a platform perspective but also from an application one. For example, SQL injection, XSS, common misconfigurations, or probing from bots and crawlers are all application-level vulnerabilities.

A web application firewall (WAF) can be implemented to help detect and prevent such application weaknesses. In Chapter 3 there is an exercise where a WAF is configured on an Application Gateway device. For now, recognize that the detection and prevention of malicious activities on the WAF is constructed by applying a rule set with the configuration. Supported rule sets are CRS 3.0, CRS 2.2.9, and OWASP CRS 3.0; the contents of each are outside the scope of this book but are fully documented elsewhere. Once you’ve determined which rule set to apply to the WAF, determine whether the requirement is to detect and log the attempt or to prevent the request from proceeding past the WAF if the occurrence is observed.

# Azure DDoS Protection

A distributed denial-of-service (DDoS) attack is when some source consumes all the available resources an application has that stops other clients or customers from getting access. There are two tiers for this product on Azure: Basic and Standard. By default, DDoS protection is enabled automatically and is constantly monitoring traffic in real time. If an attack is recognized, then the same defensive capabilities Microsoft uses to protect its online services are applied to customers running on Azure. In the Standard tier, the additional features are diagnostic and attack metric reports, resolution details if an attack happened, and the authorization to consult DDoS experts. This is a great benefit, but for most cases and purposes this protection is free.

# Azure Confidential Computing

Moving extremely sensitive data to the cloud has historically been one of the most discussed concerns customers have. It is a valid concern and one that has been taken seriously by the development of two concepts:

+   - Encryption of data at rest
+   - Encryption of data in transit

Each of those techniques helps ensure data is secured. Up to now, the security topics have mostly focused on keeping bad actors or malicious code from getting access to a resource hosted on Azure. Let’s assume this happens and that a bad actor gets access to