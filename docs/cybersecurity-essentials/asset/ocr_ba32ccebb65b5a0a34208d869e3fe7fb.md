you couldn't possibly remember. This can be a great solution so long as the password manager always works, although not every application or gateway will work with these systems conveniently.

# IP and MAC Authentication

Access control lists are commonly used by servers and routers to grant a certain amount of access. When all that's required is to ensure that access to a system is granted only to users from a particular network (or through a particular piece of hardware), IP address authentication or MAC address authentication can be effective.

However, be aware of IP address spoofing, where IP packets are created with a header containing a forged source IP address. Generally, this is done to conceal the source of a denial-of-service or other attack, but this can also be a way of defeating IP address authentication.

IP spoofing won't work in all authentication schemes, as often something must be sent back to the user and that information is sent to the spoofed IP and not the actual source of the connection. Only in the case where the “spoofer” is on the same broadcast network can the spoofer eavesdrop for the reply.

IP authentication should be used only in combination with other authentication methods or to allow a connection to merely have the opportunity to communicate with a protocol or device and not for any specific access privileges whenever possible. This is generally best used in a LAN environment where spoofing should be less of a concern.

As you learned in Part I, a MAC address can also be spoofed. While this address is embedded in the hardware device network controller, a computer can mask this address to impersonate a different device. This can be far more effective than IP spoofing because it happens before the IP connection is established. MAC address authentication may be used in combination with other authentication schemes but should not be used for any serious authentication alone.

# Authentication Protocols

A number of protocols are used on the Internet for validating access and facilitating secure communications between clients and a server-based system. These schemes are based on the use of authentication protocols. The following section discusses a few of the more common authentication protocols in use.

Password Authentication Protocol (PAP) is a standard username and password combination scheme that operates with or without an encrypted password. With both parameters set and rarely, if ever, changed, this leaves the system subject