When configuring WPA on the wireless network, note that WPA operates in the following modes:

+   - **WPA Personal**  WPA Personal is also known as WPA-PSK, which means WPA preshared key. With WPA Personal, you configure the access point with a starting key value, known as the preshared key, which is then used to encrypt the traffic. This mode is used most by home users and small businesses.
+   - **WPA Enterprise**  WPA Enterprise, also known as WPA-802.1X, is a WPA implementation that uses a central authentication server such as a RADIUS server for authentication and auditing features. WPA Enterprise is used by larger organizations so that they can use their existing authentication server to control who has access to the wireless network and to log network access.
+   - **Open**  An open wireless network does not require any password to connect and does not use any form of encryption to keep the wireless data secret from prying eyes. Naturally, it is not recommend to leave your wireless network open (you should implement WPA2) or to connect your client system to an open network that you are not familiar with.

**Although the Security+ certification exam will test your knowledge of WEP, WPA, WPA2, WPA3, and open networks, be sure to implement WPA2 or WPA3 in the real world because they are the more secure protocols.**

![](bbox=[0.109, 0.461, 0.222, 0.527])

![](bbox=[0.112, 0.556, 0.518, 0.7])

# WPA2

WPA2 improves upon the security of WPA and should be used instead of WPA if you have the choice. WPA2 uses *Counter Mode with Cipher Block Chaining Message Authentication Code Protocol (CCMP or CCM Mode Protocol)* for data privacy, integrity, and authentication on a WPA2 wireless network. WPA2 uses CCMP with the *Advanced Encryption Standard (AES)* protocol for encryption of wireless traffic instead of TKIP and also supports additional features such as added protection for ad hoc networks and key caching. Because WPA2 uses AES as its encryption protocol, it supports 128-bit, 192-bit, and 256-bit encryption.

![](bbox=[0.112, 0.797, 0.215, 0.859])

**To see how to configure WPA2, watch the video included in the online resources that accompany this book.**