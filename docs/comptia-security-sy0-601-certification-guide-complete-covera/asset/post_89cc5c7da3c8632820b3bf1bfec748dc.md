
## Practical Exercise 2 – Encrypting Data with EFS and Stealing Certificates

To encrypt data with EFS and stealing certificates, perform the following steps:

- 1. Go to the desktop. Create a folder called `test`.
- 2. Inside the folder, create a text document called `data`.
- 3. Right-click the text document called `data`, and then select **Properties**.
- 4. On the **General** tab, click **Advanced**, and then check the box against **Encrypt contents to secure data**. The data file should turn green. This means it is encrypted with EFS.
- 5. Go to the **Start** button, type `mmc`, and then select the icon with the red suitcase.
- 6. Console 1 should open. Select **File | Add/Remove Snap-in**, select **Certificates**, select **Add**, click **Next**, and then **Finish**.
- 7. Expand **Certificates | Current User | Personal**. You should see an entry for an EFS certificate.
- 8. Right-click the certificate. Select **All Tasks | Export**.
- 9. The Certificate Export Wizard appears. Press **Next**. On **Export Private Key**, select **Yes, export the private key**, and press **Next**. You will see that it is the P12 format. Press **Next**, check the **Password** box, enter the password `123` twice, and then press **Next**. In the file to export, call it `PrivKey` and save it to the desktop. Click **Next** and then **Finish**.
- 10. A box telling you the export was successful should appear.
- 11. Repeat the exercise and export the public key as `PubKey`.
- 12. You should notice the two files on the desktop. The public key has a `.cer` extension and looks like a certificate. The private key has a `.pfx` extension and looks like a letter being inserted into an envelope.

## Practical Exercise 3 – Revoking the EFS Certificate

To revoke the EFS certificate, perform the following steps:

- 1. Go to **Server Manager | Tools** and select **Certificate Authority**.
- 2. Expand **Issued Certificates** and you should see an EFS certificate.
- 3. Right-click the certificate and select **All Tasks**.
- 4. Revoke the certificate.

You will now notice that it has moved from **Issued Certificates** to **Revoked Certificates**.

## Review Questions

Now it's time to check your knowledge. Answer the questions, and then check your answers, which can be found in the *Solutions* section at the end of the book:

- 1. What type of certificate does a CA have?
- 2. If you are going to use a CA internally, what type of CA should you choose?
- 3. If you want to carry out B2B activity with third-party companies or sell products on the web, what type of CA should you use?
- 4. Why should you take your CA offline when not in use?
- 5. What type of encryption does PKI use?
- 6. Who signs X509 certificates?
- 7. What can you use to prevent your CA from being compromised and fraudulent certificates from being issued?
- 8. If two entities want to set up a cross-certification, what must they set up first?
- 9. What type of trust model does PGP use?
- 10. How can you tell whether your certificate is valid?
- 11. If the CRL is going slowly, what should you implement?
- 12. Explain certificate stapling/OCSP stapling.
- 13. What is the process of obtaining a new certificate?
- 14. What is the purpose of the key escrow?
- 15. What is the purpose of the HSM?
- 16. What is the purpose of the DRA, and what does it need to complete its role effectively?
- 17. How can you identify each certificate?
- 18. What format (PKCS) is a private certificate, and what file extension does it have?
- 19. What format (PKCS) is a public certificate, and what file extension does it have?
- 20. What format is a PEM certificate?
- 21. What type of certificate can be used on multiple servers in the same domain?
- 22. What type of certificate can be used on multiple domains?
- 23. What should you do with your software to verify that it is original and not a fake copy?
- 24. What is the purpose of extended validation of an X509?
- 25. What type of cipher is the Caesar cipher, and how does it work if it uses ROT 4?
- 26. What is encryption, and what are the inputs and outputs called?
- 27. What type of encryption will be used to encrypt large amounts of data?
- 28. What is the purpose of Diffie-Hellman?
- 29. What is the first stage in asymmetric encryption?
- 30. If Carol is encrypting data to send to Bob, what key will each of them use?
- 31. If George encrypted data four years ago with an old CAC card, can he decrypt the data with his new CAC card?
- 32. If Janet is digitally signing an email to send to John to prove that it has not been tampered with in transit, what key will they each use?
- 33. What two things does a digital email signature provide?
- 34. What asymmetric encryption algorithm should you use to encrypt data on a smartphone?
- 35. What should you use to encrypt a military mobile telephone?
- 36. Name two key-stretching algorithms.
- 37. Explain how key stretching works.
- 38. What is the difference between stream and block cipher modes, and which one will you use to encrypt large blocks of data?
- 39. What happens with cipher block chaining if you don't have all of the blocks?
- 40. If you want to ensure the integrity of data, what should you use? Name two algorithms.
- 41. If you want to ensure the protection of data, what should you use?
- 42. Is a hash a one-way or two-way function, and is it reversible?
- 43. What type of man-in-the-middle attack is SSL 3.0 (CBC) vulnerable to?
- 44. Define Diffie-Hellman Ephemeral (DHE) and Elliptic Curve Diffie-Hellman Ephemeral (ECDHE).
- 45. What are the strongest and weakest methods of encryption with an L2TP/IPSec VPN tunnel?
- 46. What is the name of the key used to ensure the security of communication between a computer and a server or a computer to another computer?
- 47. What should you do to protect data-at-rest on a laptop?
- 48. What should you do to protect data-at-rest on a tablet or smartphone?
- 49. What should you do to protect data-at-rest on a backend server?
- 50. What should you do to protect data-at-rest on a removable device, such as a USB flash drive or an external hard drive?
- 51. What protocols could you use to protect data in transit?
- 52. How can you protect data-in-use?
- 53. What is the purpose of obfuscation?
- 54. What is the purpose of perfect forward secrecy?
- 55. What type of attack tries to find two hash values that match?
- 56. What is the purpose of rainbow tables?
- 57. Explain the concept of steganography.
- 58. What are the two purposes of Data Loss Protection (DLP)?
- 59. What is the purpose of salting a password?

## 3 Investigating Identity and Access Management

Controlling access to computer systems is a key duty of any security professional. We will look at the different types of access control so that you can select the best solution for your company and, in an examination, choose the best method for a given scenario.

In this chapter, we will look at different types of authentication, looking first at Identity and Access Management concepts.

We will cover the following exam objectives in this chapter:

- Understanding Identity and Access Management Concepts
- Implementing Authentication and Authorization Solutions
- Summarizing Authentication and Authorization Design Concepts
- Common Account Management Policies

### Understanding Identity and Access Management Concepts

One of the first areas in IT security is giving someone access to the company’s network to use resources for their job. There are four key elements to **Identity and Access Management (IAM)**, and these are identity, authentication, authorization, and accounting. Let’s look at each of these in the order that they should be presented:

- **Identify**: Each person needs some form of identification so that they can prove who they are; this could be a username, smart card, or some sort of biometric control. It needs to be unique to the person using that form of identity.
- **Authentication**: The second part after proving your identity is to provide authentication for that identity. This can be done in many ways; for example, inserting a password or if you have a smart card, it would be a **Personal Identification Number (PIN)**.
- **Authorization**: Once the individual has been authenticated, they are given an access level based on their job role. This could also be known as their permission level to the system. This is based on their account type and group membership.
- **Accounting**: Computer systems maintain a log of when users log in and log out, and accounting is the process of maintaining these log files. This could be the security log in a Windows desktop in Event Viewer or it could be a database on a AAA server that is responsible for authentication, authorization, and accounting. Examples of these are RADIUS, DIAMETER, or TACACS+. These are described in more depth later in this chapter.

### Identity Types

An **identity provider (IdP)** is an entity that can validate the credentials that are presented. The identification could be a certificate, token, or details such as a username or password. IdP is used by cloud providers who use federation services to validate the identity of a user. An example of this is that they would use SAML to pass credentials to the IdP to validate their identity.

**Example:** A user authenticates using a token from a provider such as OKTA. The cloud provider uses SAML to pass the credentials back to OKTA to verify the user’s identity.
