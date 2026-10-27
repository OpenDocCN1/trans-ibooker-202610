# Managing a Public Key Infrastructure

6. ☑ B. Certificates are valid only for a certain period of time (typically one or two years) before they need to be renewed.
☒ A, C, and D are incorrect. A revoked certificate cannot be renewed, but a suspended certificate can be reactivated.

7. ☑ D. If there was a security compromise involving the certificate or the certificate was obtained in a fraudulent manner, the certificate may be revoked by the CA.
☒ A, B, and C are incorrect. These are not reasons to revoke a certificate.

8. ☑ A. The certificate revocation list (CRL) is a published list of certificates that have been revoked.
☒ B, C, and D are incorrect. A URL is an address for a web site. Key escrow is when you give a copy of keys to a third-party company so that it can decrypt information if needed. M of N control is putting a minimum restriction on the number of persons needed to recover keys.

9. ☑ C. Key escrow is giving a third party keys that can be used to decrypt company information.
☒ A, B, and D are incorrect. M of N control is putting a minimum restriction on the number of persons needed to recover keys. A CRL is a list of certificates that have been revoked by the CA. Key archiving is the backing up of keys so that they can be restored if needed.

# Implementing a Public Key Infrastructure

10. ☑ B. SSL uses a symmetric key. This is a randomly generated key used by the session to encrypt the communication between the client and server.
☒ A, C, and D are incorrect. These are not used to encrypt the web page content.

11. ☑ D. The message digest, not the message itself, is encrypted with the sender’s private key. Digital signing is not intended to maintain confidentiality (which would require encrypting the message).
☒ A, B, and C are incorrect. The message itself is not encrypted with the sender’s private key when digitally signing, and the sender’s public key is not used to encrypt the message or the message digest.