As you can see, configuration-related vulnerabilities can cause serious problems for Azure environments. Throughout this book, we will see multiple examples of these issues at the public-facing and "internal" platform configuration levels.

While this is only one example, there are additional service-specific configuration vulnerabilities that we will highlight throughout this book. Next, we will look at some additional vulnerability categories that may apply to our external-facing services.

# Patching-related vulnerabilities

While these issues are less commonly found, there are still plenty of new security patches released every week. As a result, there are plenty of forgotten or abandoned services, applications, and virtual machines that miss these patches. For this book, we will be focusing on exploiting Azure configuration-related vulnerabilities, but it's good to know the basics of exploiting patch-related issues.

From an external perspective, we will typically be scanning Azure virtual machines, with public IPs, for internet-facing services. These services can typically be fingerprinted by network scanning tools to identify the specific version of the software hosting the service.

Once the version of the software has been identified, we can either use vulnerability scanners (Nessus, Nexpose, and so on) or some basic Google skills to find potential vulnerabilities in these services.

Since this is not a network penetration testing book, we recommend checking out one of the many penetration testing books from Packt: https://www.packtpub.com/catalogsearch/result/?q=penetration%20testing.

# Code-related vulnerabilities

The "cloudification" of existing applications and the drive toward cloud-native development are some of the driving forces bringing organizations into the Azure cloud. By porting existing or legacy applications into a cloud environment, many organizations are just bringing along many of the old vulnerabilities that previously existed in their original applications.

While there are many benefits to moving to the cloud, it is not a magic bullet to fix application vulnerabilities. Some of the existing common application issues can actually become more impactful in a cloud environment, as the platform can then be abused by these issues to escalate access.