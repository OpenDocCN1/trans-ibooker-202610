If Kerberos authentication fails, this is normally down to the user’s computer or device time clock being out of sync with the domain controller by 5 minutes or more. A Network Time Protocol (NTP) server can be placed on your LAN to keep the domain computers and servers in sync with each other.

A TGT session is where a user sends their credentials (username and password, or it could be a smart card and PIN) to a domain controller that starts the authentication process and, when it has been confirmed, will send back a Service Ticket that has a 10-hour lifespan. This service ticket is encrypted and cannot be altered:

![](bbox=[0.12, 0.355, 0.875, 0.445])

Figure 3.10 – TGT session

# Single Sign-On/Mutual Authentication:

Kerberos provides a single sign-on as the user needs to log in only once. It then uses their Service Ticket to prove who they are. This is exchanged for a Session Ticket, with the server that they want to access resources on. In the example here, the user will use their service ticket for mutual authentication with a mail server:

![](bbox=[0.125, 0.625, 0.875, 0.825])

Figure 3.11 – Mutual authentication