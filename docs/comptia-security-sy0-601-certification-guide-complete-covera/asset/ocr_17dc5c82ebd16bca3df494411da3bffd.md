The preceding diagram shows the logged-in user exchanging their encrypted **Service Ticket** with the mail server, which, in return, provides mutual authentication by returning a **Session Ticket**. The logged-in user checks that the session ticket's timestamp is within 5 minutes of that of the domain controller. This means that Kerberos can complete mutual authentication.

> **Important note**
> 
> You need to remember that Kerberos is the only authentication protocol that uses tickets. It will also prevent replay attacks as it uses USN numbers and timestamps. It can also prevent pass-the-hash attacks as it stored the passwords in an encrypted database called ntds.dit.

**NT Lan Manager (NTLM):** NTLM is a legacy authentication protocol that stores passwords using the MD4 hash that is very easy to crack. It was susceptible to the pass-the-hash attack. It was last used in a production environment in the 1990s. Kerberos prevents pass-the-hash attacks as it stored the passwords in an encrypted database called ntds.dit.

# Transitive Trust

Transitive trust is where you have a parent domain and maybe one or more child domains; these are called **trees**. Refer to the following diagram:

![](bbox=[0.2, 0.55, 0.77, 0.843])

Figure 3.12 – Transitive trust