
## Mesh Topology

The mesh network design offers the most basic network connection scheme and the most extensive management scheme. In this topology, each node has a direct physical connection to all the other nodes in the network. Mesh topologies are employed in two very large network environments—the public telephone system and the Internet, as well as in small, wireless piconet communication networks that include Bluetooth and ZigBee networks.

Piconets are small, short-range networks of devices grouped together to communicate on the same channel. One of the devices operates at the master device, or coordinator, while the other devices operate as slaves. The role of master device can circulate through the network with different devices assuming that role as it becomes available.

## Logical Topologies

It would be easy to visualize the connections of the physical topologies just described if the nodes simply connected to each other. However, this is typically not the case in most network arrangements. This is due to the fact that most networks employ connectivity devices, such as hubs, switches, and routers, which alter the appearance of the actual connection scheme. Therefore, the logical topology will not match the appearance of the physical topology—the particulars of the connection scheme are hidden inside the connecting device.

As an illustration, Figure 12.5 shows a typical network connection scheme using a connecting device to consolidate connections in a single location. The physical topology appears as a star. However, the internal wiring of the connecting router provides a logical bus topology. It is not uncommon for a logical ring or mesh topology to be implemented in a physical star topology.

![](img/2459befd842d6fed299fabe6da3512d0_301_0.png)

**FIGURE 12.5** Logical Topologies

Unlike SSL, IPsec operates at Layer 3 in the OSI model and secures everything in a network. IPsec also differs from SSL in that it is implemented on a client system, whereas SSL is built into a browser. All of this combined leads to a safer environment for transferring data, usually at a faster rate. There are two modes of IPsec:

- ► Transport mode – The IP payload is encapsulated only.
- ► Tunnel mode – The entire IP packet is encapsulated.

Connection Security includes authenticating two computers prior to communication and then ensuring the confidentiality between them. Windows employs IPsec to achieve Connection Security.

## Procedure

In this procedure, you will discover how and where IPsec is configured. You will create a new Connection Security rule, and explore the options associated with IPsec. This procedure is intended to instruct you on how to create an IPsec communication; however, you will not be able to test it in this environment.

## Accessing IPsec

IPsec is located within Windows Firewall with Advanced Security. It has been renamed Connection Security; however, it is generally stated that Connection Security is achieved by using IPsec rules.

- 1. Power on your machine.
- 2. Log on to Windows using your administrative account.
- 3. At your desktop, in the search bar embedded in your taskbar, type wf.msc and press Enter.
- 4. This should launch the Windows Firewall with Advanced Security.
- 5. Click the Connection Security Rules entry, as illustrated in Figure 12.6. The middle pane should have no current rules established.
- 6. Expand Monitoring and then Security Associations by selecting the arrow next to each, as illustrated in Figure 12.7.

![](img/2459befd842d6fed299fabe6da3512d0_304_0.png)

**FIGURE 12.6** Windows Firewall with Advanced Security

![](img/2459befd842d6fed299fabe6da3512d0_304_1.png)

**FIGURE 12.7** Expanded Monitoring and Security Associations Items

5. Read the definition of each and select Request Authentication For Inbound And Outbound Connections, as illustrated in Figure 12.9.

![](img/2459befd842d6fed299fabe6da3512d0_307_0.png)

FIGURE 12.9 New Connection Security Rule Wizard – Requirements Window

6. Click Next to view the Authentication Method window.
Although four options are listed, the two involving Kerberos V5 are restricted to connections made within an Active Directory forest.

7. Select the radio button next to Advanced to make the Customize button available, as shown in Figure 12.10.

8. Click the Customize button to launch a separate Customize Advanced Authentication Methods window, as shown in Figure 12.11.

![](img/2459befd842d6fed299fabe6da3512d0_308_0.png)

FIGURE 12.10 The Customize button is active.

In this window, you can customize a list of authentication methods to fit your needs. Explore the window carefully.

9. Click the Add button located under First Authentication Methods: The Add First Authentication Method window will appear, as shown in Figure 12.12.

# FIGURE 12.11 Customize Advanced Authentication Methods Window

# FIGURE 12.12 Add First Authentication Method Window

This Add First Authentication Method window offers four possible authentication methods from which to choose:

- - Kerberos V5
- - NTLMv2
- - Certificate
- - Preshared key (not recommended)

10. After exploring the options, click Cancel to exit the window. Click Cancel in the Customize Advanced Authentication Methods window as well.

11. You should be returned to the New Connection Security Rule Wizard – Authentication Method window. Select the radio button next to Default and then click Next. You will be directed to the Profile window.

These selections relate to the network connections in place on the computer. While a standalone computer in a networked office may not need to worry about using a Public network, a traveling laptop will need to ensure integrity and confidentiality over the Internet.

12. Leave all three check boxes selected, as shown in Figure 12.13, and click Next to move to the Name window.

13. Enter Request IPsec Rule under Name. Leave the Description (Optional): blank, as shown in Figure 12.14.

14. Click Finish to save the rule. You are brought back to the Windows Firewall With Advanced Security window. The new rule is now located in the middle pane, as illustrated in Figure 12.15.

Notice the right pane. The Actions panel has changed and more options are available.

To create an IPsec connection, another computer would configure a Connection Security rule using the same process. Any time the computers want to transfer information, an IPsec connection needs to be completed.

# FIGURE 12.13 New Connection Security Rule Wizard – Profile Window

Recall that you selected the Request Authentication, Not Required option. Therefore, you can still connect to anyone else without a secure connection.

- 15. Select the rule in the middle pane to highlight it. In the right pane, click the Properties entry, as shown in Figure 12.16.

Each tab allows you to adjust the settings you selected in the New Connection Security Rule Wizard.

- 16. Explore each tab, and then click the Cancel button to exit the Properties window.

![](img/2459befd842d6fed299fabe6da3512d0_311_0.png)

- - Encapsulating Security Payloads (ESP) offers origin authentication as well as encryption. ESP encrypts and encapsulates the entire TCP/UDP datagram within an ESP header that does not include any port information. This means that ESP won’t be able to pass through any device using port address translation (PAT).
- - Security Associations (SAs) offer a number of algorithms and frameworks for authentication and key exchange.
- - Internet Key Exchange (IKE) is the protocol used to set up a security association in IPsec.

- 2. Unlike SSL, IPsec operates at Layer 3 in the OSI model and secures everything in a network. IPsec also differs from SSL in that it is implemented on a client system, whereas SSL is built into a browser. All of this combined leads to a safer environment for transferring data—usually at a faster rate. The options include:
  - Request authentication, but do not require.
  - Require authentication for inbound and outbound.
  - Require inbound, request outbound. A hybrid of the previous options.

- 3. The IPsec standard uses a suite of protocols to accomplish its goal of securing data. AH gives integrity while ESP provides both integrity and confidentiality.
  - Integrity – Confirms that the data packets have not been tampered with. Prevents man-in-the-middle attacks.
  - Confidentiality – Confirms that the data packets cannot be viewed by a malicious user.
  - Prevents network sniffers from viewing data.

4. Connection Security authenticates two computers prior to communication and then ensures the confidentiality between them. Windows employs IPsec to achieve connection security.
