found that through validation a control that was assumed to be present was not. In this case, the security team and the development team will review their threat modeling process and develop better process documentation as well as ensure that the appropriate stakeholders are included in the threat modeling process.

# 4.2.4 DevOps supports security better

DevSecOps, SecDevOps, DevOps with Security, it’s been called many things. I’ve mostly heard it referred to as DevSecOps, so that is the term I will use here. Like the parent term of DevOps, the DevSecOps definition can depend on the process and technology.

> **DEFINITION** IBM defines DevSecOps as automatically baking in security at every phase of the software development lifecycle, enabling development of secure software at the speed of Agile and DevOps.

The methodologies I covered previously each provide a means for security to be integrated, however the DevOps methodology provides a unique opportunity for security to be more rapidly combined with other features in the development pipeline. As described, DevOps, and by extension DevSecOps, allows for development teams to have security fixes deployed to a production environment as quickly as the CI/CD pipeline can support.

In today’s environments, where your applications are running, security issues are ever present. In early 2021, Redscan released a report that reviewed the trends in vulnerabilities that were added to the National Vulnerability Database (NVD) from 1989 to 2020. This report highlighted how much has changed over that time and specifically how 2020 was a banner year for new vulnerabilities. The report found the following:

+   - More security vulnerabilities were disclosed in 2020 (18,103) than in any other year to date – at an average rate of 50 CVEs per day
+   - 57% of vulnerabilities in 2020 were classified as being ‘critical’ or ‘high severity’ (10,342)
+   - There were more high and critical severity vulnerabilities in 2020 than the total number of all vulnerabilities recorded in 2010 (4,639 including low, medium, high, and critical)
+   - Nearly 4,000 vulnerabilities disclosed in 2020 can be described as ‘worst of the worst’ – meeting the worst criteria in all NVD filter categories
+   - Low complexity CVEs are on the rise, representing 63% of vulnerabilities disclosed in 2020

What does this tell us about what we are up against in application security? Vulnerabilities are being released at a rapid pace and they are becoming easier to exploit. This means that in order stay ahead of significant issues, application security needs to be able to move swiftly as well. With this in mind, the methodology that the engineering organization has decided to use matters. For instance, in the Waterfall methodology, waiting possibly weeks or even months to release a resolution to a found vulnerability would leave the organization exposed much longer than necessary. Even in an Agile organization, a fix might be weeks away. Pretty fast, but not fast enough for an attacker that might already