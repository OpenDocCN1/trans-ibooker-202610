is used to control access to the wireless network, the wireless client attempts to connect to a WAP; the AP asks the client for proof of identity and then forwards that to a RADIUS server for authentication.

Many variations of EAP have developed over time:

+   - **LEAP** Lightweight Extensible Authentication Protocol is Cisco’s proprietary EAP solution created before the IEEE created 802.1X.
+   - **PEAP** Protected Extensible Authentication Protocol is used to encapsulate EAP messages over a secure tunnel that uses Transport Layer Security (TLS). Because EAP assumes the packets are sent over a secure network, with PEAP, TLS is used to create a secure tunnel between two points.
+   - **EAP-FAST** EAP-FAST is an authentication protocol designed by Cisco to replace LEAP. EAP-FAST is typically used to provide authentication services to wireless networks.
+   - **EAP-TLS** EAP-TLS is the EAP protocol that uses TLS security for secure authentication on wireless networks. The EAP-TLS solution typically involves the use of client certificates to perform the authentication.
+   - **EAP-TTLS** EAP-TTLS (EAP-Tunneled Transport Layer Security) builds on EAP-TLS by having the capabilities to authenticate both the client and the server, although the client does not need to use certificates for authentication. The server can authenticate the client after a secure channel is set up using the server’s certificate.

When configuring WPA on the wireless network, note that WPA operates in three different modes—WPA Personal, WPA Enterprise, and Open:

+   - **WPA Personal** With WPA Personal, aka WPA-PSK (WPA preshared key), you can configure the AP with a starting key value, known as the preshared key, which is then used to encrypt the traffic. This mode is used most by home users and small businesses.
+   - **WPA Enterprise** WPA Enterprise, aka WPA-802.1X, is a WPA implementation that uses a central authentication server such as a RADIUS server for authentication and auditing features. WPA Enterprise is used by larger organizations so that they can use their