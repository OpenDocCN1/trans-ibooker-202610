# WPA3

In 2018, a new wireless security protocol was created known as WPA3, which improves on the security of WPA2. Like the previous WPA security protocols, WPA3 supports personal and enterprise mode. With personal mode it uses CCMP-128, which uses AES-128 (128-bit encryption with AES in CCM mode), but in enterprise mode it uses 192-bit encryption.

WPA3 has been improved by using a number of security features such *Simultaneous Authentication of Equals (SAE)*, which increases security by allowing the access point to authenticate the client as well as the client to authenticate the access point. This improvement to the authentication security helps prevent the cracking of handshake traffic that is common with WPA2.

Another major improvement is that WPA3 uses session keys in the encryption process, which helps prevent one user from decrypting another user’s traffic even though the wireless password/key used by both users is the same.

# RADIUS Federation

Another common security technology for authentication with wireless networks is *RADIUS federation*. With RADIUS federation, two or more organizations who trust each other can configure their RADIUS servers for mutual authentication. For example, if an employee of CompanyX is visiting an office of CompanyY and attempts to log on, the servers at CompanyY forward the authentication request to CompanyX. After the RADIUS server at CompanyX authenticates the user and generates a token, that token can then be used to access resources on the network of CompanyY due to trust set up between CompanyX and CompanyY.

# CERTIFICATION OBJECTIVE 9.02

# Securing a Wireless Network

When I purchased my first wireless router several years ago, I knew nothing about wireless and was concerned that I would be unable to set up the wireless network. My concerns were quickly dismissed when I took the wireless router (access point) out of the box and gave it power. My laptop connected almost immediately! I was amazed at how easy it was to connect to the wireless network—there was *no* configuration required. Then it dawned on me—if my laptop connected with no configuration, what is stopping the rest of the world from connecting to my wireless network? The answer, of course, is that nothing is stopping someone from sitting outside my yard and connecting to my wireless network. I needed to figure out how to stop unauthorized access to my wireless access point.

You can use various techniques to prevent unauthorized persons from connecting to your wireless network. You may want to implement some or all of these features. To help secure your wireless infrastructure, consider changing settings on the router such as the