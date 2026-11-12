# THE 9 A.M. PROBLEM

Donald Farmer

I began working with commercial enterprise servers in the 1990s. Previously my work had focused on complex, but smaller, scenarios, often in research. But when designing reporting and analytics applications for large clients such as oil companies, retailers, and health care providers, I became familiar with a problem of scale. At 9 a.m. each weekday, and especially on Mondays, almost every manager would log in to the system and refresh their critical reports. This was the peak load on the system, and we quickly learned that simply throwing resources at the problem—such as larger servers—was not a popular solution, mainly because it was so expensive. We used a variety of techniques, including predictive preprocessing, caching, cache sharing, and just-in-time processing, to make it work.

You may not have this problem today. In fact, in these days of cloud computing you should not. Your cloud service provider should be able to provision truly elastic scalability that is efficient and cost-effective. However, you should take care to model this scenario—calculating what peak demand will look like—and ensure that it is indeed reliably implemented by your cloud provider and analytics engine.

# Security

In Chapter 6 we will dig deeper into the security implications of embedded analytics, and in Chapter 7 we will also look at some topics relating to data governance. Here, it is important to point out some key criteria to consider when choosing a platform:

+   - Ensure the analytic system can implement the right protocols for your needs. Look for standard integrations such as Lightweight Direct Access Protocol (LDAP), OAuth, or Active Directory.