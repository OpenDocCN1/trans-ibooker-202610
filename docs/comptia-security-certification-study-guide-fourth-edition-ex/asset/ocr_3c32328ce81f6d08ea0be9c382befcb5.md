# FIGURE 10-5 802.1X communication phases

![](bbox=[0.1, 0.21, 0.87, 0.34])

# 802.1X Stages of Communication

When preparing for the Security+ certification exam, it is important to know the stages of 802.1X communication and the protocols used at each stage. Figure 10-5 depicts the following stages of communication between the 802.1X components:

+   1. **EAPOL Start** Communication starts with the client machine, or supplicant, sending an EAPOL Start message. This is a layer-2 frame requesting access to the switch or wireless access point.
+   2. **EAPOL Identity Request** The switch or wireless access point, also known as the authenticator, sends an EAPOL frame back to the supplicant requesting the supplicant identify itself.
+   3. **EAPOL Identity Response** The supplicant sends an EAPOL response message that includes its authentication information. This could be a username and password or it could be a certificate.
+   4. **Credentials sent** The authenticator uses RADIUS to send an IP packet containing the credentials to the authentication server.
+   5. **RADIUS Access: Accept/Reject** The authentication server sends back a RADIUS Access message to the authenticator that includes either an accept or reject status. If the credentials were verified, it sends an Access-Accept message, and if the credentials were incorrect, it sends an Access-Reject message.
+   6. **EAPOL Success/Fail** Finally, the authenticator sends an EAPOL message to the supplicant with a success or fail status. With a Success message, the client system is granted access to the network.

# Claims-Based Authentication/Federation Services

Federation services is an important topic nowadays, and one you are expected to know for the Security+ certification exam. Federation services give you the capabilities of single sign-on, which (as described earlier) means that once you log on and a token is granted, you