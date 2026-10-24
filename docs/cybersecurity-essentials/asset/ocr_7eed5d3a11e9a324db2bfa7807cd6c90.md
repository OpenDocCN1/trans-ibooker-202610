At some point we may need to consider security checkpoints and keypads, etc. With a large number of internal users in different departments accessing the network, we need to make sure the network is segmented using switches to facilitate the creation of access control procedures throughout the entire network.

Clearly a substantial firewall router will be needed. Here we will disallow all IP traffic from the WAN (Internet) side of things and then only allow specific services and ports that we may need.

If public access is needed to a server, then we need to create a perimeter network or DMZ and move those functions to a server located within that network. If they main firewall cannot handle an additional interface, then a simpler firewall is a good idea for the perimeter network as well, so that we can both provide VPN access to the public server(s) but also restrict the services that can enter the DMZ.

All authorized users must use an encrypted VPN to access DMZ or internal network assets. Once a user has VPN access, then other assets must be protected with access control using credentials or IP-based controls. Ideally your VPN can assign users to a unique IP address, so that you can protect and monitor the network on a per-user basis by IP and therefore only rarely have to resort to credentials.

For the DMZ firewall or interface we will only enable the ports needed, such as 80 and 443 for a web-based system and the minimum mail services ports we need to make available. Ideally mail is only made available via VPN, but if a web-mail service is made available on the DMZ server and that server is responsible for accepting mail, then the appropriate ports must be opened.

All sensitive information stored on the DMZ server must be stored encrypted. If any breach does occur you want to ensure the data is protected by some good encryption.

For the network firewall or interface we hopefully can disallow every service and only allow VPN access. We then create firewall rules that restrict access to the various network segments by the users’ VPN IP address. Turn off ICMP and other unnecessary services on the firewall as well. Even though we have restricted access to the network, it is still a good idea to lock down every service on the server as well.

# Detect

Our firewall router or appliance is going to play an important role in protection but not so much with detection. DMZ assets should be locked down as tight as