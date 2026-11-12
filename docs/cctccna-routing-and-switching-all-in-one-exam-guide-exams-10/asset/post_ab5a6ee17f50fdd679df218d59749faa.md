
![](img/0eafdcebe219ef3756f0e57bcf1eb997_929_0.png)

**EXAM TIP** The CCNA exam expects you to know these different authentication factors, but it also expects you to identify *single-factor authentication*, *two-factor authentication*, and *three-factor authentication* schemes.

In addition to the preceding factors, you should be familiar with several other factors of authentication. If you are authenticating to a system by using only one of the authentication factors, you are using a *single-factor authentication* scheme. For example, knowing the username and password is a single-factor authentication scheme because they are both examples of something you know.

A *two-factor authentication scheme* is based on two authentication factors, such as something you know and something you have. For example, you have a bank card (something you have) and you know the PIN to use the bank card in an ATM. This authentication scheme is much stronger than simply needing to have the bank card to gain access to your account. If that were the case, then anyone with the bank card could withdraw money from the account. The following are common examples of two-factor authentication schemes:

- **Physical token and password** You authenticate with something you have and something you know.
- **Smartcard and PIN** You authenticate with something you have and something you know.
- **Biometrics and password** You authenticate with something you are and something you know.
- **Logon credential, and then access code from mobile phone notification**
  In this example you would log on with your username and password, and then a notification is sent to your phone with a one-time access code you will need to type when logging into the application. This is an example of something you know (username and password), and something you have (the phone).

Multifactor authentication (MFA) requires you to log in with two or more factors. Suppose you need an authentication code to log into a site. You enter your username and password (something you know), and you use your phone (something you have) to receive an authentication code. Once you receive the code, you enter that on the site to complete the login process. This is an important feature that should be used with any web site login to ensure that if someone is able to get your password they cannot log into the site as you because they do not possess your phone.

Although two-factor authentication is the more common form of MFA, you can have more such as three-factor authentication. An example of three-factor authentication is if you are required to do a retina scan (something you are), swipe a card (something you have), and then insert the PIN associated with that card (something you know).

![](img/0eafdcebe219ef3756f0e57bcf1eb997_930_0.png)

**EXAM TIP** For the exam, know that a smartcard is an example of two-factor authentication, because you need to have the smartcard and you need to know a PIN to gain access. Another example of two-factor authentication is a fingerprint scan and a password. Notice that in this example, the two factors are something you are and something you know.

## Single Sign-On

Single sign-on (SSO) is an important concept regarding authentication. When you authenticate to the network that offers SSO, you can access multiple systems based on that single authentication information. So with SSO, you are not required to authenticate with each different system you access—you authenticate once and then can gain access to multiple systems without authenticating again.

Using SSO, you don’t have to authenticate with each server individually. Years ago, before SSO was available, logging on to each system required a separate username and password, so users had to remember various passwords to access more than one server. With SSO, the user logs on with one set of credentials, a token is generated, and the token is then presented to the other trusted servers on the network to enable the user to access resources on the network.

![](img/0eafdcebe219ef3756f0e57bcf1eb997_931_0.png)

**EXAM TIP** For the Cisco CCNA exam, remember that authentication occurs when someone identifies herself to the system and her identity is verified.

## Authorization

Once you have been authenticated to the network, you will be authorized to access network resources. There are various types of authorization, depending on what it is that you are trying to be authorized for. For example, if you are trying to access a file on the network, authorization is determined according to the permissions assigned to the file. If you are trying to change the time on the server, authorization is determined by your privileges, or rights. From a Cisco networking point of view, authorization is implemented as access control lists, which are configured on the router to allow or deny traffic from entering or leaving the network.

## Permissions vs. Rights

Permissions and rights are examples of authorization used on endpoints such as workstations and servers. (This is not a Cisco example, but I wanted to mention it because it is an example of authorization, and one that you will come across on the network if you are responsible for managing security on servers.)

A *permission* is your level of access to a resource such as a file, folder, or object. The permission is a characteristic of the resource and not a characteristic of the user account. For example, if an administrator wanted to give Bob read permission to a file, the admin would go to the properties of that file and set the permissions. Notice that the admin does not go to the user’s account to assign the permissions.

In Windows operating systems, a *right* is your privilege within the operating system to perform a particular task. For example, when companies deploy Windows to all client systems on the network, users may be surprised that they cannot change the time on their computers. This is because they do not have the Change System Time rights. A user (or a group to which a user is a member) must be assigned the appropriate right to configure a particular aspect of the operating system. Rights are a significant security feature of the Microsoft OS. It is important to note that administrators can configure the OS because the group that they are a member of (Administrators) is assigned all the rights.

The goal here is not to make this too much of a Windows discussion, but just to offer examples of authorization. After you log on to a computer, you are authorized, or not authorized, to access different files or perform different actions because of the permissions and rights that have been configured for you as a user.

## Controlling Traffic

A Cisco example of authorization is the access control list (ACL). To authorize which traffic is allowed to enter or leave different parts of your network, you will configure rules on your firewall or Cisco routers. The ACL is a list of these rules that either permit or deny different types of traffic from entering or leaving your network. (You will learn more about ACLs in Chapter 17). The router receives a packet and then typically allows or denies the packet based on the protocol, the source or destination IP address (IP filtering), or the source and destination port (port filtering).

## Network Access Control

Network access control (NAC) is a hot technology today that enables you to authorize who can gain access to a wired or wireless network based on the state of the connecting system, known as *posture assessment*. With NAC, you can specify conditions that a system must meet in order to gain access to the network. If those conditions are not met, the user can be sent to a restricted network, where he can take actions to remedy the problem.

For example, you may require that for a system to connect to the network, it must have antivirus software installed, and the antivirus definitions must be up-to-date. You may also require that the system have a personal firewall enabled. If any of these conditions is not met, the NAC system places the client on a restricted network, where the user can typically apply patches or, in this case, perform an update of the virus definitions.

When connected to the restricted network, the client has no access to network resources because communication to the private company network from the restricted network is controlled. Figure 16-2 displays a network access control environment.

![](img/0eafdcebe219ef3756f0e57bcf1eb997_933_0.png)

Figure 16-2 NAC is used to place health requirements on systems connecting to the network.

Today, NAC is used in many scenarios, as demonstrated in the following examples:
