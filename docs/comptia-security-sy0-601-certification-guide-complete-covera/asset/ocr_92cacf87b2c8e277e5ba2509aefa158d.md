The way it is stored in the Active Directory can be viewed using a tool called **ADSI Edit**:

![](bbox=[0.305, 0.12, 0.66, 0.42])

Figure 3.9 – ADSI Edit

LDAP is the Active Directory responsible for storing the X500 objects. When the Active Directory is searched, then LDAP provides the information required. LDAPS is the secure version of LDAP.

Here are some examples.

+   - **Example 1:** If I want to know how many people are in the IT OU, I can search the Active Directory. LDAP provides the search and returns a reply saying that the IT department has 10 members.
+   - **Example 2:** I am searching the Active Directory for a user called Fred. Once again, LDAP finds the user. If you have 10,000 people in your domain, you will have them in different OUs to make it easier to find and manage them. However, if you need to find someone, this will still be difficult. That is why we need LDAP to perform the search. It saves time.

# Kerberos

Kerberos authentication uses tickets, **Updated Sequence Numbers (USN)**, and is time-stamped. The process of obtaining your service ticket is called a **Ticket Granting Ticket (TGT)** session. The times on all servers and computers must be within 5 minutes of each other; time can be synchronized by using a time source such as the Atomic Time clock.