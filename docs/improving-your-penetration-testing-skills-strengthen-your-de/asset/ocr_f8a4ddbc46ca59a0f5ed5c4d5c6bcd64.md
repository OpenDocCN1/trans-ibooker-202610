[*] Time: 2017-10-23 09:12:50 UTC Vuln: host=192.168.216.10 name=Nessus SYN scanner refs=NSS-11219

# Integrating with NeXpose

In the previous recipe, we discussed Nessus as a potential vulnerability scanner. In this recipe, we will cover another important vulnerability scanner called NeXpose.

NeXpose is a popular tool by Rapid7, which performs the task of vulnerability scanning and importing results to the Metasploit database. The usage of NeXpose is similar to Nessus, but let's have a quick look at how to get started with NeXpose. I will leave the task of exploring it deeper as an assignment for you.

# Getting ready

You can download NeXpose Community from http://www.rapid7.com/products/metasploit/metasploit-community-registration.jsp. After installing NeXpose, you can start using it the from the msfconsole, but first, we need to load the plugin to connect to the NeXpose server. Let's execute these steps in the command line:

![](bbox=[0.19, 0.48, 0.81, 0.68])

To connect with the NeXpose server, use the nexpose_connect command followed by the credentials, hostname, port, and verify the SSL certificate:

```
msf > nexpose_connect NexposeUser:NexposeP4ssw0rd@127.0.0.1:3780 ok
[*] Connecting to Nexpose instance at 127.0.0.1:3780 with username NexposeUser...
msf >
```