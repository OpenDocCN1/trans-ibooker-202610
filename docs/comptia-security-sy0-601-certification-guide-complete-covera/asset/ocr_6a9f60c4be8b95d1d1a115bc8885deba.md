# Chapter 2 – Implementing Public Key Infrastructure

+   24. The US released The CLOUD Act so that they could obtain evidence from other countries for an FBI investigation. The UK government released the COPOA act to seek data stored overseas and give their law enforcement faster access to evidence held by providers.
+   25. In Stage C of Cloud Forensic Process 26, you ascertain the type of technology behind the cloud.

+   1. A CA has a root certificate, which it uses to sign keys.
+   2. You would use a private CA for internal use only; these certificates will not be accepted outside of your organization.
+   3. You would use a public CA for B2B activities.
+   4. If you were a military, security, or banking organization, you would keep the CA offline when it is not being used to prevent it from being compromised.
+   5. PKI uses asymmetric encryption.
+   6. The CA signs the X509 certificates.
+   7. Certificate pinning can be used to prevent a CA from being compromised and fraudulent certificates from being issued.
+   8. If two separate PKI entities want to set up cross-certification, the root CAs would set up a trust model between themselves, known as a bridge trust model.
+   9. PGP uses a trust model known as a web of trust.
+   10. A Certificate Revocation List (CRL) is used to determine whether a certificate is valid.
+   11. If the CRL is going slow, you should use OCSP as it provides faster validation.
+   12. Certificate stapling/OCSP stapling is where a web server uses an OCSP for faster certificate authentication, bypassing the CRL.
+   13. You would submit a Certificate Signing Request (CSR) to request a new certificate.
+   14. The key escrow stores and manages private keys for third parties.