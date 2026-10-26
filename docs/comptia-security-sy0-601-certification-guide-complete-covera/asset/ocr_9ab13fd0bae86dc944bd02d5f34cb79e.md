# Cloud vs. On-Premises Vulnerabilities

On-premises is where a company is solely responsible for all risks and vulnerabilities, as well as assuring they have the appropriate physical measure in place to handle them and protect the company. They will have an enterprise network firewall, NIPS, and NIDS to protect the network and may employ security guards to ensure that only the authorized personnel gain access. In a Cloud environment, the Cloud Service Provider (CSP) is responsible for the security of the cloud. In an IaaS environment, the CSP provides the hardware and its security, but it is the customer that is responsible for the installation of software, configuration, and patching of the systems.

# Zero Day Virus

Even if the company has an effective patch management system, they could suffer from a Zero Day virus as it cannot be detected by any of your monitoring systems until an effective patch has been produced. The reason it is called Zero Day is that it happens on Day 0. It might take a vendor until Day 4 before they can capture it and provide an effective solution against it. A heuristic or anomaly-based NIDS or NIPS should be used so that they may detect the new variants.

# Weak Configurations

There are various types of weak configurations that need to be avoided on all devices within your environment:

+   - **Open Permissions:** In this type, everyone has the same access levels to the same data. Most administrators are not experienced with the cloud environments and could also inadvertently set the wrong permissions to their data. Amazon Web Services (AWS) S3 storage were the most common crime scene according to Paolo Passeri, Cyber Intelligence Principal at Netskope. https://www.ceotodaymagazine.com/2020/02/heres-one-of-amazons-biggest-weak-spots/
+   - **Unsecure Root Accounts:** In a Linux environment, the root account is an administrator account that has access to all commands and files. Some developers use this account for their development purpose, but the root account is only as safe as their password. If the password is weak, then the account is not secure.
+   - **Errors:** For security purposes, It is important that when an application is created, it is set to only display generic errors to the user so as to avoid revealing potential weaknesses in coding errors. This is why secure coding practices are so vital.