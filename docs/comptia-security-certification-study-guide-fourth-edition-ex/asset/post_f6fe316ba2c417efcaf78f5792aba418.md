
## Accountability

A trend in recent years is to designate the A in CIA to represent both availability and accountability (or to add an A to represent accountability, CIAA). Accountability is ensuring that users are accountable for their actions—if someone inappropriately deletes a file, for example, a record of that action exists to hold them accountable.

You implement accountability in the organization by implementing auditing and logging features on the systems, routers, firewalls, and in the applications. The concept here is that if you log the activity and are able to identify who caused a certain event to occur, you can hold that person accountable for their actions. The following are some popular methods to implement accountability within the organization:

- **Log files** Most network services either implement logging by default or can be configured to log activity to log files. Be sure to enable logging for all core services on the network so that if an incident arises, you can review the logged data.

- **Audit files** Most operating systems have a security-auditing feature that allows you to review the security-related events that occur on a system. In Windows, this is the security log in Event Viewer. Be sure to review the security audit logs on a regular basis.

- **Firewalls and proxy servers** Most firewalls and proxy servers can log outbound user activity, such as web sites that are visited and applications used for outbound communication. Be sure to review the firewall and proxy server logs on a regular basis to hold the users accountable for their actions.

- **Application logging** It is becoming more important to log activity within applications. For example, if someone deletes a customer record or a purchase from the purchasing system, you want to know about it. Find out what levels of logging are available in all your critical applications, and let users know you are logging activity. This will help keep them accountable for their actions within the business software. For example, Microsoft SQL Server has features that allow the database developers to implement logging and auditing in the database application.

> exam watch
> The Security+ exam expects you to understand CIA and different methods of implementing confidentiality, integrity, and availability.

## EXERCISE 2-1: CIA Scenarios

In this exercise, you will read the following scenarios and identify which goal of information security is being satisfied (confidentiality, integrity, availability, or accountability).

| Term | Scenario |
|---|---|
| ______ | A. You have configured auditing on your SQL server so that if a user deletes a customer record, the information is recorded in the audit log. |
| ______ | B. You have configured BitLocker To Go on a USB drive for Bob to be able to store files on the USB drive in an encrypted format. |
| ______ | C. Sue has configured the e-mail server in a server cluster with another server so that if one of the servers fails, the other server can handle the workload. |
| ______ | D. You place the current budget spreadsheet on the company intranet server so that employees can download the file. You publish the hash value of the file on the web site as well. |
| ______ | E. Critical tasks are divided into different jobs, with each person performing one of the different jobs. |

## CERTIFICATION OBJECTIVE 2.02: Understanding Authentication and Authorization

A big part of securing your environment is to ensure that you implement some form of identifying and verifying individuals within the organization and then control what resources, web sites, and areas of the facility they have access to.

This section is designed to introduce you to the concepts of authentication and authorization. Note that each of these topics has its own chapter dedicated to it, but I want to make sure you have a basic understanding of the concepts and terminology in this chapter.

### Identification and Authentication

Identification happens before authentication and is the process of having users identify themselves to the system. The most popular method companies use to identify individual users is to give each user a unique username. The users type their username into the system in order to identify themselves.

After the user inputs the identifying information (the username), the user inputs the password for that account for purposes of authentication. The information is then sent to an authentication system that is responsible for verifying that the username and password are valid. If the username and password are correct, the user is granted access to the system, but if the information is incorrect, an error is displayed and access is denied.

Users can identify themselves and authenticate to the system in a number of ways. The following lists a few popular methods used for identification and authentication purposes:

- **Username** The most popular method of identifying users on the network is to give them each a unique username. For the users to identify themselves, they type the username into a logon screen. To be authenticated, they type the password associated with that username.

- **Smartcard** A smartcard is the size of a credit card and has a microchip that can contain data used by a system or application (see the left side of Figure 2-3). The smartcard can be used to identify the unique owner of that specific smartcard. Once the smartcard is inserted into the system, the user types a PIN associated with the smartcard in order to authenticate to the system.

- **Token** A security token is a small device that is typically used to identify an individual and is used in the authentication process. Of the different types of tokens, the most popular is a device that displays a random number on it for 30 to 60 seconds (see the right side of Figure 2-3). The random number, username, and password are used to log on.

![](img/3e3af33e2c753e920c09ecd40b5a457a_99_0.png)

**FIGURE 2-3** A smartcard (left) and a SecurID token (right) can be used during authentication.

- **Biometrics** Biometrics is the concept of using part of your physical self to authenticate to the system. For example, you can scan a fingerprint or a retina to authenticate to a system. Highly secure environments often use biometric systems because physical characteristics cannot be replicated.

> exam watch
> For the exam, know that before you can be given access to resources (authorization), you must first identify yourself to the system. Your identification information is then verified against an authentication database to verify that you can gain access to the system or facility (this is known as authentication).

### Authorization

Once the user has been authenticated, they are given access to different resources; this is known as authorization. You have many ways to authorize individuals for different resources. The following are some examples of authorization:

- **Permissions** You may authorize individuals to access a file by giving them permission to the file or giving a group the individual is a member of permission to the file. This is one of the most popular methods of authorization.

- **Router ACLs** Another example of implementing authorization is by configuring access control lists (ACLs) on a router. These ACLs determine whether the router is allowed to accept certain traffic and route it to a different network.

- **Proxy servers** Another popular example of authorization is allowing or denying access to different web content at the proxy server. The proxy server is a server on the network that all traffic headed out to the Internet passes through. The proxy server can control what web sites can be visited or even what types of Internet applications can be used by the internal users.

- **Facility** A final example of authorization is to control access to different areas of the building. For example, Bob's smartcard may give him access to the area of the building that he works in, but the card does not open doors to other areas of the building—he is not authorized to access those areas.
