During the first login to your server, which is now a domain controller, you will notice at the login screen that you are now signing in with a domain account, no longer a local account on this server. Domain controllers don't actually have local user accounts anymore; if you were using the built-in administrator account to sign in to your server before, like I was, that account and password have now been converted into the domain administrator account. This account can now be used to log in to any computer or server that is later joined to your domain and will automatically receive administrative rights on that device. Make sure you're using a good domain administrator password, and don't share it with anybody!

# Multiple domain controllers for redundancy

Now, having configured our first domain controller, we should jump in and start setting up objects inside Active Directory, right? Not yet! I'm being a little dramatic here, of course you can jump ahead and start using the tools that interface with AD, which we will discuss immediately following this section of our chapter. But if you are setting up a real environment and not just monkeying around in a test lab, it is going to be super critical that you establish redundancy in your domain.

Adding a second domain controller is even easier than spinning up the first one. Simply take the same steps that you did last time, with one exception to the way that the NIC is configured:

+   1. Spin up a new server.
+   2. Set a static IP.
+   3. Configure a good permanent hostname.
+   4. Deviation from before: Instead of configuring your NIC's DNS server settings to point at the new server itself, point your primary DNS to the existing domain controller's IP address.