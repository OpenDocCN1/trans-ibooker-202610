I will go ahead and press 2 on my keyboard and then press Enter, and I am immediately presented with a prompt asking me to specify a new computer name. This is an extremely fast way to configure the hostname of new Server Core servers. Since I don't actually want to rename WEB4, I will leave the selection blank and press Enter to return to the main screen.

Now I want to check out the networking settings. Pressing 8 and then Enter brings me into Network Settings, where I can see that my current IP address on WEB4's NIC is 10.10.10.12. This is correct, but let's change that address for the sake of walking through a real Sconfig setting change.

I first selected my network adapter index, which was number one. I am now shown additional information about what is already configured on this NIC and have options to change this information. Selecting option one again will allow me to set the network adapter address:

![](bbox=[0.11, 0.36, 0.87, 0.79])

Figure 10.30: Configuring an IP address with Sconfig