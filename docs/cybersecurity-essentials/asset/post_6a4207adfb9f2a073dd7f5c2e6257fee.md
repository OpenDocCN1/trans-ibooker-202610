
+   3. Click Run in the dialog box.

+   4. Click Accept And Install when the installer appears, as shown in Figure 21.16. Allow the software to make changes if necessary.

![](img/2459befd842d6fed299fabe6da3512d0_542_0.png)

FIGURE 21.16 Using the Installer

+   5. If there is a message to close Edge, as shown in Figure 21.17, click Skip Import.

![](img/2459befd842d6fed299fabe6da3512d0_542_1.png)

FIGURE 21.17 Skipping the Import

+   6. Open a new tab by pressing Ctrl+T.

+   7. Close the other two tabs by clicking on the X on the tabs you want to close.
+   8. Your Opera browser should look similar to Figure 21.18.

![](img/2459befd842d6fed299fabe6da3512d0_543_0.png)

# FIGURE 21.18 Opera Browser

+   9. Click on the Settings icon in the top-right corner. This icon looks like a set of three sliders.
+   10. Scroll to the bottom and select Go To Browser Settings. This should bring up a window similar to that shown in Figure 21.19.
+   11. Click Privacy & Security in the Settings window.
+   12. Scroll down to VPN section, as depicted in Figure 21.20.
+   13. Click on the Enable VPN check box. You may notice that a blue VPN icon has appeared in the address bar.
+   14. In the address bar, type https://www.askapache.com/online-tools/whoami.

Your Internet connection may seem slower. This is normal when using a VPN. The VPN creates more routing and encapsulation requiring more time and bandwidth to facilitate communications. If this website doesn’t work, you can try using the keyword “whoami” in an Internet search engine.

![](img/2459befd842d6fed299fabe6da3512d0_544_0.png)

**FIGURE 21.19**  The Browser Settings Gear

![](img/2459befd842d6fed299fabe6da3512d0_544_1.png)

**FIGURE 21.20**  The VPN Settings

+   15. Examine the information displayed; see Figure 21.21 for an example.  
+       The `REVERSE_DNS` listing is the most important information.

![](img/2459befd842d6fed299fabe6da3512d0_545_0.png)

**FIGURE 21.21** The Results of Apache “Whoami”

+   16. Scroll down and click on Show My Information.

After clicking, you should see a map and a guess of your location.

+   17. To verify the information provided by a second source, navigate to www.google.com and type where am i in the query box. You should see results similar to those depicted in Figure 21.22. Note, if you allow Google to use geolocation services, this step might indeed show your true location.

+   18. Perhaps this location is in fact near you. Click on the VPN icon at the left side of the address bar. Click on the words “Optimal Location” to change where Internet websites think you are. Choose a location far away from you. (We chose a location in Asia, as shown in Figure 21.23.)

+   19. Click on the search bar and press Enter to request “where am i.”

Where are you now? Your reported location should be different, although it could be nearby.

+   20. Close your browser.

## Protecting Data Moving Through the Internet

As previously discussed, most users and organizations want to use the Internet to interact with different types of people. These interactions can involve activities such as exchanging information through email, social media, FTP transmissions, and other transport mechanisms. They can also involve activities that include sending sensitive personal information, conducting financial activities, or transferring medical information that typically calls for heightened security measures. In this chapter, you will learn to:

- Describe typical Internet access control strategies, including authentication methods and protocols
- Define basic cryptography implementations used to secure data in motion, including encryption, digital signatures, certificate management, PKI, public versus private keys, hashing, and key management
- Describe the two major components involved in creating a virtual private network (VPN)

## Securing Data in Motion

Because the Internet is public and based on TCP/IP, strangers and bad people can potentially examine these interactions. So, transmission-specific security features must be added to these messages to protect the data while it is in motion between the secure environment of the sender and the secure environment of the intended receiver.

The NIST Cybersecurity Framework standards specifically call for establishing security for data in motion (as opposed to data at rest, which is stored in some device that may be more protected). The following sections of this chapter deal with devices, tools, protocols, and policies involved in securing data as it moves across the Internet.

Often, security is implemented as we initially configure services. Sometimes we really don’t even think of it as security per se but rather simple configuration. It is important to be thinking about security throughout the configuration process because this is typically where many security features are enabled. To properly implement security concepts, it is important that you have a basic understanding of authentication and encryption.

## Authentication

As illustrated in all of the previous chapters, security at all levels always involves some type of authentication process. Recall that authentication is simply a way to know the identity of a user through some means. You will certainly want to authenticate outside users who want to access your network, and you will generally also want to authenticate internal users as they venture out to locations unknown on the Internet.

Users can be authenticated by asking them to provide a username and password, examining their MAC address, or by their network address if they are assigned a static IP address. A combination of factors may be used, and it is becoming more common to involve other factors unrelated to the network in the authentication scheme. If unauthenticated users are allowed on the network, you will have no way of tracking their activities.

After a user is authenticated, you will generally want to determine their authorization, which essentially involves the resources that the authenticated user has permission to access, and what actions they can perform. Often, these credentials and permissions are stored in some sort of database, but sometimes authentications are stored in simple text files. The security of these files and databases are critical aspects of network security.

After a user has been authenticated and authorized, their activities need to be accounted for by means of some type of logging process. This is an often-overlooked concept related to authentication. However, logging is a key component of the security process, as security is never a purely preventative activity. Administrators must wade through logs from time to time to evaluate what events have happened, might have happened, or may yet happen in their networks, as well as to whom it happened.

## Single-Factor Authentication

From the previous three chapters, you should be aware that the most well-known authentication method is certainly password authentication. It can be as simple as prompting the user for a password and nothing else. This is commonly seen when wireless users connect to an access point. The user is able to browse available access points around them and then only needs to provide a password or authentication key to connect. This is single-factor authentication and represents the lowest level of security available.

Single-factor authentication may be fine for restricting access to resources but can make it more challenging to verify the identity of the actual user. It may also be sufficient for guest access in a residential network, but this conveys a certain amount of trust in those users because they can now access the network and its Internet connection with much the same access as the owner.

You can configure restrictions, but at some point users will have access capabilities that increase your liability. When that liability is too great, you must find another way to authenticate. If passwords are a part of your security plan, you also must consider policies related to those passwords. A poor password may be enough for home networks, but every business should have strict policies relating to password quality and perhaps even password history.

## Multifactor Authentication

Two-factor authentication involves asking for authentication using a second, different method. For example, one factor is something physical or in the user’s possession such as:

- An RFID key
- A USB key dongle
- A card swiped

Another factor is some physical characteristic, such as:

- A fingerprint or iris scanned
- A spoken phrase, analyzing the user’s voice

The different factors used can be set apart as one of the following:

- Something you know (a password or PIN)
- Something you have (a physical token or card)
- Something you are (physical characteristic like fingerprint)
