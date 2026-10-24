possible, but generally if some intrusion does happen the intruder will be looking for certain PHP exploits and trying repeatedly to log in with simple credentials. Security software can be configured to look for such things and block the access and notify an administrator.

Simple down detection is a great idea for both the internal and DMZ servers with a protected asset polling critical servers and notifying an administrator when they become unavailable or even overloaded.

I suggest using an open-source intrusion detection system (IDS) or maybe even more than one to assist in this process. Products such as Snort and Tripwire can help detect intrusion early on so that it can be dealt with in a timely manner. Linux packages such as Security Onion can provide an arsenal of tools that can supplement the capabilities that you may have with your security appliance or firewall.

# Respond

When creating policies and policy statements, assume your audience is non-technical and keep the jargon to a minimum. They will need to understand certain concepts such as VPNs, but they may need to be educated along the way about why some issues are important. Your network is more secure when your users understand security better.

IT staff will need to be educated about intrusion notifications and given strict policies on how to proceed when a notification is received. If you are using a mitigation service for DDoS attacks, then policies related to how to proceed during an attack are important as well. These same IT staff must also be educated to provide user-friendly advice to the network users, so they understand how to handle certain more subtle attacks such as phishing requests.

Education is so very important and so you should create educational materials to supplement the network policies and procedures. Your users need to understand why good passwords are important and why they should be suspicious of anything that could be a phishing attack and why they should not share credentials with anyone, etc.

Make sure you have robust backup procedures in place, ideally backup to servers not available to the network per se. The backup servers may access the servers but not the other way around, so minimize the impact of any intrusion. Cloud based and offsite backup mechanism should ideally supplement onsite backups.

# Recover

If you are experiencing a denial of service type of attack and do not use a mitigation service, then you should disconnect the network from the Internet while