If I now choose to add a new role from inside Server Manager that is aware of multiple servers in the network, when I get to the screen asking me where I want to install that role, I see that I can choose to install a new role or feature onto one of my other servers, even though I am not working from the console of those servers, as shown in Figure 2.22:

![](bbox=[0.13, 0.18, 0.87, 0.51])

Figure 2.22: Selecting a server to install a new role or feature on

If I wanted to install the AD DS role onto DC2, a server I'm prepping as a second domain controller in my environment, I would not have to log into the DC2 server. Right here, from Server Manager running on CA1, I could run through the Add Roles wizard, define DC2 as the server that I want to manipulate, and install the role directly from here.

# Remote Server Administration Tools (RSAT)

Using Server Manager on a single server to manage and monitor all of your servers is pretty handy, but what if we could take one more step out of that process? What if I told you that you didn't have to log into any of your servers, but could perform all of these tasks from the computer sitting on your desk?

This is possible by installing a toolset from Microsoft called the Remote Server Administration Tools (RSAT). I have a regular Windows 10 client computer online and running in our network, also domain-joined. I am now going to add an optional feature to this Windows 10 computer to give it the RSAT toolset.