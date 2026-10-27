Renewal Before your certificate expires, you can have it renewed so that you can use the certificate for a longer period. Certificates are typically renewed for one- or two-year periods. If you do not renew a certificate and the validation period passes, then the certificate is unusable by applications, because they check this validation period setting before using a certificate.

Suspension and revocation If at any point the CA determines you have been fraudulent in your request for or use of a certificate, the CA may decide to suspend or even revoke your certificate. A suspended certificate is no longer usable, but it can be placed back into an active state easily. A revoked certificate is permanently unusable (more on revoked certificates in the next section).

Destruction/expiration As part of the certificate life cycle, you need to determine how the certificates and related keys will be destroyed after their validation time has expired. You want to ensure that hackers do not get their hands on any of the keys used in the PKI structure so that they cannot attempt to reuse them and impersonate a user or device. Also note that certificates have a validation period associated with them, and after that period the certificate is considered expired. An expired certificate is no longer valid and can no longer be used.

# Certificate Revocation Lists and OCSP

As noted, your certificate can be revoked by the CA at any time if the CA determines that you were fraudulent in obtaining the certificate or there has been a security compromise related to your certificate.

The CA is responsible for creating the certificate revocation list (CRL), which is a list of certificates that have been revoked. The CRL is published to a web site at regular intervals, and applications download the CRL to verify that a certificate has not been revoked before using the certificate.

> exam Watch The Security+ exam is sure to test your knowledge of the CRL. Know that the CRL is published by the CA on a regular basis and that applications will check the CRL to verify that a certificate being used has not been revoked.

Another method that systems and applications can use to verify whether a certificate has been revoked relies on the Online Certificate Status Protocol (OCSP). OCSP is an Internet protocol that uses HTTP to communicate with the CA and check the status of a certificate.