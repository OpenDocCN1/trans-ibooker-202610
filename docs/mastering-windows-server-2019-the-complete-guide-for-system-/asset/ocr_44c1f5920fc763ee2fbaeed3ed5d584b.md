# Install the AD DS role

Now that the server is prepped to be a domain controller, we install the role! Since you are already familiar with the process of adding roles to servers, we won't step through the whole thing again. Simply utilize Server Manager or PowerShell to install the following components onto your new server:

![](bbox=[0.11, 0.2, 0.87, 0.44])

Figure 3.2: Installing Active Directory Domain Services and DNS Server roles

As mentioned, I am going to install AD DS for Active Directory, and DNS Server so that this domain controller can also serve as a true DNS server for our new business. As you progress through the role installation wizard, you will encounter some informational text that is useful to read over, but there should be no further options that you need to select in order to get both of these roles installed onto your server.