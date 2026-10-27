
State/province ON

Country/region CA

![](img/3e3af33e2c753e920c09ecd40b5a457a_608_0.png)

+   6. Click Next on the Cryptographic Service Provider Properties screen.
+   7. Type c:\certreq.txt as the filename for the request and then click Finish.

## Submit the Certificate Request to the CA

Use the following steps to submit the request to the CA. (In this example, the web site and the CA are the same system, but in real life they will be different systems.)

+   8. On the ServerA VM, launch a web browser and type http://10.0.0.1/certsrv/ to send the request for the certificate to the CA.
+   9. Log in with your administrator’s username and password.
+   10. Choose the Request a Certificate link.
+   11. Choose the Advanced Certificate Request link to request a web server certificate.
+   12. Choose the second link to submit the certificate request file (the one labeled “Submit a certificate request by using a base-64-encoded CMC or PKCS #10 file or Submit a renewal request by using a base-64-encoded PKCS #7 file”).

## Test the Site

You will now test the site to ensure that you are required to use https instead of http:

+   28. Go to the Windows 10 VM and start a web browser.
+   29. Navigate to http://10.0.0.1. You should get an error. What does the error say?
+   30. Connect to the SSL port on the site using https://10.0.0.1 to verify that you can surf the SSL-enabled web site.

## Managing a PKI

You need to perform a number of tasks when implementing and maintaining the PKI environment. How you perform these tasks will depend on the software you're using. The following sections provide a general idea of how to perform these management functions.

### Revoke a Certificate

As mentioned previously, at any point in time, the CA can revoke a certificate if it determines the certificate was obtained in a fraudulent manner or if there was a security compromise involving the certificate. The following steps can be performed to revoke the certificate:

+   1. From Server Manager, choose Tools | Certification Authority.
+   2. Select the Issued Certificates folder.
+   3. Right-click the certificate you wish to revoke and choose All Tasks | Revoke Certificate (see Figure 13-5).
+   4. Choose a reason for revoking the certificate from the drop-down list and then click Yes.

### FIGURE 13-5

Revoking a certificate

![](img/3e3af33e2c753e920c09ecd40b5a457a_611_0.png)

### Publish the CRL

After the certificates have been revoked, the CA can publish the CRL to a distribution point where applications check the CRL. Follow these steps to publish the CRL to a distribution point:

+   1. Within the Certification Authority console, right-click Revoked Certificates and choose All Tasks | Publish.
+   2. Choose the type of CRL to publish:
    - New CRL  Choose this to publish a new, complete version of the CRL.
    - Delta CRL  Choose this to publish just the updates to the existing CRL.
+   3. Click OK.

The CRL is published to the distribution point that is specified as a server setting. Follow these steps to verify or modify the location of the distribution point:

+   1. In the Certification Authority console, right-click the server and choose Properties.
+   2. Choose the Extensions tab.
+   3. Ensure the CRL Distribution Point (CDP) extension is selected (see Figure 13-6).
+   4. You can add or remove distribution points with the Add or Remove button.

### FIGURE 13-6

Specifying CRL distribution points

![](img/3e3af33e2c753e920c09ecd40b5a457a_612_0.png)

### Implement Recovery Agents

To create recovery agents for your PKI structure, the first thing you will need is a recovery agent certificate for individuals acting as recovery agents. You can obtain the certificate by creating a certificate request and choosing the Recovery Agent template.

Once the recovery agent certificate is created, you can specify the recovery agent in the CA server settings as a recovery agent for certificates that are flagged for key archival.

Perform the following steps to implement recovery agents with a Windows CA:

+   1. In the Certification Authority console, right-click the server and choose Properties.
+   2. Choose the Recovery Agents tab and select Archive the Key (see Figure 13-7).
+   3. Specify the number of recovery agents allowed.
+   4. Click Add to add the certificates of the recovery agents and then click OK.

### Implement Key Escrow

Remember that key escrow is allowing a third party to recover cryptography keys, so great care should be taken when implementing key escrow. You also want to be sure to specify the strict conditions under which the third party is allowed to participate in key recovery. For example, it may be company policy that a court order is needed for the third party to recover cryptography keys.

### FIGURE 13-7

Specifying
recovery agents

![](img/3e3af33e2c753e920c09ecd40b5a457a_613_0.png)

## Implementing a Public Key Infrastructure

10. Using SSL, the web page content is encrypted with which of the following?

+   A. Server’s public key
+   B. Random symmetric key
+   C. Server’s private key
+   D. Client’s private key

11. When digitally signing a message, which of the following is true?

+   A. The message is encrypted with the sender’s private key.
+   B. The message is encrypted with the sender’s public key.
+   C. The message digest is encrypted with the sender’s public key.
+   D. The message digest is encrypted with the sender’s private key.

### Performance-Based Question

12. You are configuring your certificate server and want to use a hashing algorithm that creates 160-bit hash values. Using the exhibit, identify the hashing algorithm you would use.

![](img/3e3af33e2c753e920c09ecd40b5a457a_617_0.png)

## SELF TEST ANSWERS

### Introduction to Public Key Infrastructure

+   1. ☑ B. The registration authority (RA) is responsible for validating the certificate request and the entity requesting the certificate.
      ☒ A, C, and D are incorrect. The root certificate authority (CA) and subordinate CA are used to create certificates after the request has been validated by the RA. The certificate revocation list (CRL) is a list of revoked certificates that is published by the CA.
+   2. ☑ B, C, and E. The certificate is an electronic file that binds the entity to the private key. The certificate also stores information such as the public key, validation period for the certificate, and the CA that issued the certificate.
      ☒ A and D are incorrect. The client IP address and server IP address are not associated with the certificate.
+   3. ☑ D. The major drawback to using your own private CA is that applications do not trust certificates generated by your CA by default. You can install the CA's certificate into the trusted CA list to change this.
      ☒ A, B, and C are incorrect. Certificates obtained from public CAs cost money, but those obtained from private CAs do not. Both public CA and private CA certificates will need to be renewed, and certificates from a private CA are not trusted by default.
+   4. ☑ C. The root CA should be disconnected from the network and potentially powered off so that it cannot be compromised.
      ☒ A, B, and D are incorrect. These are not common practices that should be performed with a root CA.
+   5. ☑ D. The PEM certificate file format is an ASCII file that contains the text -----BEGIN CERTIFICATE----- and -----END CERTIFICATE-----.
      ☒ A, B, and C are incorrect. CER and PFX are incorrect because they are binary files and not ASCII files. P7B is an ASCII file, but it does not have the text -----BEGIN CERTIFICATE----- and -----END CERTIFICATE----- within the file.

### Managing a Public Key Infrastructure

6. ☑ B. Certificates are valid only for a certain period of time (typically one or two years) before they need to be renewed.
☒ A, C, and D are incorrect. A revoked certificate cannot be renewed, but a suspended certificate can be reactivated.

7. ☑ D. If there was a security compromise involving the certificate or the certificate was obtained in a fraudulent manner, the certificate may be revoked by the CA.
☒ A, B, and C are incorrect. These are not reasons to revoke a certificate.

8. ☑ A. The certificate revocation list (CRL) is a published list of certificates that have been revoked.
☒ B, C, and D are incorrect. A URL is an address for a web site. Key escrow is when you give a copy of keys to a third-party company so that it can decrypt information if needed. M of N control is putting a minimum restriction on the number of persons needed to recover keys.

9. ☑ C. Key escrow is giving a third party keys that can be used to decrypt company information.
☒ A, B, and D are incorrect. M of N control is putting a minimum restriction on the number of persons needed to recover keys. A CRL is a list of certificates that have been revoked by the CA. Key archiving is the backing up of keys so that they can be restored if needed.

### Implementing a Public Key Infrastructure

10. ☑ B. SSL uses a symmetric key. This is a randomly generated key used by the session to encrypt the communication between the client and server.
☒ A, C, and D are incorrect. These are not used to encrypt the web page content.

11. ☑ D. The message digest, not the message itself, is encrypted with the sender’s private key. Digital signing is not intended to maintain confidentiality (which would require encrypting the message).
☒ A, B, and C are incorrect. The message itself is not encrypted with the sender’s private key when digitally signing, and the sender’s public key is not used to encrypt the message or the message digest.
