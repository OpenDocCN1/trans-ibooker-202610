
## SELF TEST

The following questions will help you measure your understanding of the material presented in this chapter. As indicated, some questions may have more than one correct answer, so be sure to read all the answer choices carefully.

### Understanding Wireless Networking

1. Which wireless mode involves two laptops connecting directly to one another?
   - A. Infrastructure mode
   - B. Ad hoc mode
   - C. Laptop mode
   - D. Enterprise mode

2. Which of the following wireless standards does not fall into the Wi-Fi standard?
   - A. 802.11n
   - B. 802.11g
   - C. 802.11b
   - D. 802.11a

3. Which wireless standard runs at 54 Mbps at the 2.4-GHz frequency?
   - A. 802.11n
   - B. 802.11a
   - C. 802.11g
   - D. 802.11b

4. Which wireless standard can reach transfer rates of up to 600 Mbps?
   - A. 802.11n
   - B. 802.11a
   - C. 802.11g
   - D. 802.11b

5. Which wireless security protocol changes the key using TKIP?
   - A. WEP
   - B. WPA
   - C. WEP2
   - D. WPA5

6. Which wireless security protocol uses CCMP and AES to perform encryption of the communication?
   - A. WEP
   - B. WPA
   - C. WEP2
   - D. WPA2

7. You would like to implement a wireless security protocol that supports SAE authentication between the wireless client and wireless access point. What wireless security protocol would you use?
   - A. WPA2
   - B. WPA
   - C. WPA3
   - D. WEP

### Securing a Wireless Network

8. Which of the following is the name you assign to your wireless network?
   - A. MAC address
   - B. Service Set Identifier (SSID)
   - C. WEP key
   - D. IP address

9. What should you do with the wireless router to help hide the wireless network from unauthorized users?
   - A. Turn it off when it is not being used.
   - B. Enable WEP.
   - C. Disable SSID broadcasting.
   - D. Unplug the network cable from the router.

10. You wish to encrypt traffic between the wireless client and the access point, but you don’t have a wireless router that supports WPA or WPA2. What would you do to secure the traffic?
    - A. Use a third-party program to encrypt the traffic.
    - B. Use WPA on the client but WEP on the router.
    - C. Use WPA2 on the client but WEP on the router.
    - D. Use WEP on both the client and the router.

11. Derek is performing a wireless site survey and has found a wireless router connected to the network. Looking out the window next to where he found the wireless router, Derek also notices some strange markings on the building across the street. What is the term for these markings?
    - A. War dialing
    - B. War driving
    - C. Rogue access point
    - D. War chalking

12. Sean, a senior security officer for your company, has been monitoring wireless activity for some time and suspects that an attacker has spoofed the MAC address on their network card to gain access to the network. What wireless security feature has been compromised in this example?
    - A. MAC filtering
    - B. WEP
    - C. WPA2
    - D. SSID broadcasting disabled

13. Sue is responsible for configuring the wireless network for Jetstream, Inc. Management has expressed great concern about wireless security and would like to ensure that anyone outside the building cannot connect to the wireless network. What should Sue do?
    - A. Configure MAC filtering.
    - B. Enable WPA2.
    - C. Enable WPA.
    - D. Lower the power level.
    - E. Disable SSID broadcasting.

14. Jeff is a junior security officer for Company XYZ and is currently responsible for the security of the company's wireless network. Jeff is working on configuring the wireless encryption and wants to use the most secure encryption option with a RADIUS server for authentication. Which encryption option should he use?
    - A. WPA Personal
    - B. WPA2 Personal
    - C. WEP
    - D. WPA Enterprise
    - E. WPA2 Enterprise

15. Dan, a security engineer for your company, has recently limited which devices can connect to each port on the switch by MAC address. Which of the following wireless attack methods is Dan mitigating?
    - A. IV attack
    - B. WPA2 cracking
    - C. Rogue access point
    - D. MAC spoofing

### Configuring a Wireless Network

16. What is the first thing you should change on the wireless router when it is powered on?
    - A. The cryptographic protocol
    - B. The admin password
    - C. The IP address
    - D. The DHCP server scope

17. You have just purchased a wireless router but do not intend to have any wireless clients for the first six months. What should you do to help secure the router?
    - A. Enable WEP.
    - B. Disable SSID broadcasting.
    - C. Enable WPA2.
    - D. Disable the wireless features.

18. What program could be used to do a survey of your area and discover wireless networks? (Choose two.)
    - A. MBSA
    - B. Device Manager
    - C. Cain & Abel
    - D. Routing and Remote Access
    - E. Kismet

### Other Wireless Technologies

19. What is the transfer rate of Bluetooth 3.0?
    - A. 3 Mbps
    - B. 24 Mbps
    - C. 4 Mbps
    - D. 1 Mbps

20. What type of wireless vulnerability involves exploiting a Bluetooth device and copying data off the device?
    - A. Bluejacking
    - B. Bluetoothing
    - C. Bluesnarfing
    - D. Bluenosing

21. Tom has recently received a number of unwanted advertisements on his mobile device. What type of attack has occurred?
    - A. Bluejacking
    - B. Bluetoothing
    - C. Bluesnarfing
    - D. Bluenosing

### Performance-Based Questions

22. Using the following exhibit, match each term on the right to its corresponding description on the left.
    - Used to control which devices can connect
    - Wireless encryption using a password
    - Used to ensure someone is close to the wireless router in order to connect
    - Used to hide the wireless network
    - Wireless encryption using a RADIUS server for authentication
    - Power Level
    - MAC Filtering
    - WPA2 Enterprise
    - WPA2 Personal
    - Disable SSID Broadcasting

23. Using the following exhibit, match each term on the right to its corresponding description on the left.
    - A wireless communication protocol that can be used by devices to pay for an item in a store
    - Hacker exploits a smart phone over Bluetooth and copies data off the phone
    - A rogue access point set up by the hacker that uses the same network name as your company wireless network
    - A wireless assessment feature that plots wireless coverage and signal strength in a diagram
    - A user receives unsolicited messages over Bluetooth to their smart phone
    - Can be used by companies to have a guest user agree to terms of use before gaining access to the wireless network
    - Captive portal
    - Evil twin
    - Heat map
    - Bluesnarfing
    - NFC
    - Bluejacking

### Configuring a Wireless Network

14. ☑ E. Both WPA Enterprise and WPA2 Enterprise are wireless security protocols using a RADIUS server for central authentication, but WPA2 is the more secure of those protocols. ☒ A, B, C, and D are incorrect. WPA Personal, WPA2 Personal, and WEP do not support RADIUS server authentication. WPA Enterprise does support RADIUS server authentication, but it is not considered as secure as WPA2 Enterprise.

15. ☑ C. To help prevent rogue access points from being connected to the network, Dan can limit which devices can connect to the switch by their MAC address. This is a network security feature known as port security. ☒ A, B, and D are incorrect. None of these attack methods can be mitigated by port security.

16. ☑ B. When you first get the router, you should set the admin password to prevent others from logging on and changing your router’s settings. ☒ A, C, and D are incorrect. Although you will want to change a number of settings on the router, the admin password should be the first.

17. ☑ D. If you do not have a need for the wireless features of the router, then you should disable them. ☒ A, B, and C are incorrect. These are all settings you would set if you had wireless clients.

18. ☑ C and E. Cain & Abel is a password-cracking tool that has wireless scanner functionality that can detect wireless networks close to you. Kismet is a wireless scanner for Linux that can display the wireless networks and the clients connected to those networks. ☒ A, B, and D are incorrect. These are not wireless scanners.

### Other Wireless Technologies

19. ☑ B. The speed of Bluetooth version 3.0 is 24 Mbps. ☒ A, C, and D are incorrect. None of these represents the transfer rate of Bluetooth.

20. ☑ C. Bluesnarfing is when a Bluetooth device is exploited and data is copied or deleted off the device. ☒ A, B, and D are incorrect. Bluejacking is sending unsolicited messages to Bluetooth devices. Bluetoothing and bluenosing are fictitious terms.

21. ☑ A. Bluejacking is a Bluetooth vulnerability that involves the hacker sending unsolicited messages to a Bluetooth-enabled device. ☒ B, C, and D are incorrect. Bluesnarfing is another Bluetooth exploit that allows a hacker to retrieve data from the device. Bluetoothing and bluenosing are fictitious terms.
