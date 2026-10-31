Restarting the DNS service requires access to use the Restart-Service cmdlet, right? But doesn't that mean I would be able to restart any service on that server and could potentially do all sorts of things that I don't need to do? JEA is even powerful enough to deal with this scenario. When setting up the level of access that the user needs to get, you can dive into particular cmdlets and divide up permissions. In our example, you could provide the user with access to the Restart-Service cmdlet, but only give permissions to restart particular services, such as those pertaining to DNS. If the user tried to Restart-Service on WINrm, they would be denied.

# Disable external RDP...NOW

We all know how to RDP into servers, and indeed I bet 99% of anyone reading this RDPs into servers most days of the week. Doing so from inside the corporate network is a fast and easy way to interact with servers, and it makes a lot of sense that we employ this technology. Have you ever thought about the possibility of allowing RDP access over the internet? Wouldn't it be nice if you could simply open up MSTSC on your home computer and hop right into your servers without having to first establish a cumbersome VPN? Have you done it? Do you have servers who right now have NAT rules configured in your firewall so that you can hit a certain port using RDP from anywhere in the world and RDP directly into your server?

STOP IT RIGHT NOW!

Shut down the port, delete the NAT rule, do not pass Go, do not collect $200.

This is the absolute worst idea, and yet I discover NAT rules just like this on a very regular basis. I often work with new companies, supporting various parts of their infrastructure, and during the discovery of these client networks, we always check over firewall rules. It is truly amazing how many places in the world have RDP access to servers open to the entire internet.

Now, perhaps you're saying to yourself, "Of course I don't do that! Well, I do, but I am tricky and use ambiguous port numbers for this access. You certainly won't find RDP port 3389 open on my firewall because I set up my NAT rules so that you have to connect to crazy things like port 33343 and 5896; nobody will ever find those."

You. Are. Wrong.

It makes no difference whether your external ports are 3389 or 12345 or anything else. You might have gotten even sneakier and changed the port your servers are using to listen for RDP connections, so that 3389 is not involved either in the external or the internal part of the communication. That still makes no difference. Attackers have port scanners that can easily seek out any open RDP channels; the port those RDP channels use makes no difference to them.