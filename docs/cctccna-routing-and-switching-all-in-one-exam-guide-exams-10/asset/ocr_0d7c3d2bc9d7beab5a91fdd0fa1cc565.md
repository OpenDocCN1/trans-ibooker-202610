characteristic of the user account. For example, if an administrator wanted to give Bob read permission to a file, the admin would go to the properties of that file and set the permissions. Notice that the admin does not go to the user’s account to assign the permissions.

In Windows operating systems, a *right* is your privilege within the operating system to perform a particular task. For example, when companies deploy Windows to all client systems on the network, users may be surprised that they cannot change the time on their computers. This is because they do not have the Change System Time rights. A user (or a group to which a user is a member) must be assigned the appropriate right to configure a particular aspect of the operating system. Rights are a significant security feature of the Microsoft OS. It is important to note that administrators can configure the OS because the group that they are a member of (Administrators) is assigned all the rights.

The goal here is not to make this too much of a Windows discussion, but just to offer examples of authorization. After you log on to a computer, you are authorized, or not authorized, to access different files or perform different actions because of the permissions and rights that have been configured for you as a user.

# Controlling Traffic

A Cisco example of authorization is the access control list (ACL). To authorize which traffic is allowed to enter or leave different parts of your network, you will configure rules on your firewall or Cisco routers. The ACL is a list of these rules that either permit or deny different types of traffic from entering or leaving your network. (You will learn more about ACLs in Chapter 17). The router receives a packet and then typically allows or denies the packet based on the protocol, the source or destination IP address (IP filtering), or the source and destination port (port filtering).

# Network Access Control

Network access control (NAC) is a hot technology today that enables you to authorize who can gain access to a wired or wireless network based on the state of the connecting system, known as *posture assessment*. With NAC, you can specify conditions that a system must meet in order to gain access