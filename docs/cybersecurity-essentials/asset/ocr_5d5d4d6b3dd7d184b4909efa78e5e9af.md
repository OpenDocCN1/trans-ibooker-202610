Software such as web browsers may contain CA keys to facilitate the handling of certificates. However, they should also reference a Certificate Revocation List (CRL) or use the Online Certificate Status Protocol (OCSP) to query for revoked certificates. Increasingly, browsers are employing OCSP, but there are still those providers that simply include a CRL with each update of their browser software.

A certificate chain is the list of certificates starting with the root certificate followed by every other certificate where the issuer or signer of one certificate is the subject of the next. While a certificate can have only one issuing CA signature, different certificate chains may exist for some certificates because more than one certificate can be created using the same public key.

Secure certificates should be obtained only from known and respected CAs, or you risk having the certificates revoked. CAs are increasingly pushing Extended Validation (EV) certificates where the identity of the entity is verified much more extensively. EV certificates are no stronger or structurally different than other certificate types, but they imply a greater trust because the site being visited is owned by the certificate holder.

Web browsers typically have some visual indication that the SSL/TLS connection is using an EV certificate. This usually involves displaying information using a green color. EV certificates are typically used only on sites that accept credit cards or other private data (rather than those that handle email or other services).

It is possible to generate and use a self-signed certificate for encryption. This can be useful for testing. However, client software will inform the user that this certificate is not trusted.

# Secure Sockets Layer (SSL)

Secure Sockets Layer (SSL) is a protocol for managing both authentication and communication between clients and servers using both a public key and a private key. SSL uses the sockets method to exchange data between client and server program. A socket is bound to a specific port number and once created will listen for a connection request. Web requests using SSL use the HTTPS protocol designation rather than HTTP.

In 2014, SSL version 3.0 was considered vulnerable due to a repeatable, man-in-the-middle attack method called Padding Oracle on Downgraded Legacy Encryption (POODLE). Therefore, SSL v3.0 is now considered obsolete.

# Transport Layer Security (TLS)

Transport Layer Security (TLS) is the successor to SSL even though the two are often referred to interchangeably or even together as SSL/TLS. They are