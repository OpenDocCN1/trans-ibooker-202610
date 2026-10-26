![](bbox=[0.35, 0.09, 0.57, 0.13])

Use with 30 seconds

Figure 3.1 – TOTP

+   - **HMAC-Based One-Time Password (HOTP)**: An HOTP is similar to a TOTP in that a one-time password is issued. The main distinguishing factor is that there is no restriction in terms of time, but you can only use this password once.

Next, we will look at certificate-based authentication.

# Certificate-Based Authentication

Certificate-based authentication is very popular as it provides two-factor authentication, which makes it more secure than single-factor authentication, such as a username and password. We will now look at its various types:

+   - **Smart Card**: As previously mentioned in this book, a smart card looks like a credit card with a chip on it. The certificate is located on the chip itself and does not leave any trace (footprint) on the computer or laptop being used.
+   - **Common Access Card (CAC)**: CACs are used by governmental and military personnel as they provide both authentication and identification as it has a picture of the user on it. They are similar to smart cards. On the front side of a CAC is a picture of the user with their service (Army, Navy, or Airforce), and the reverse side shows their blood group and their Geneva Convention Category. To view a CAC card, please go to https://www.cac.mil/Common-Access-Card/.
+   - **Personal Identity Verification (PIV)**: This is very similar to the CAC but is used by federal agencies rather than the military.

Let's now look at other types of authentication.

# Port-Based Authentication

IEEE 802.1x is a port-based authentication protocol that is used when a device is connected to a switch or when a user authenticates to a wireless access point. Authentication is normally done by certificate.

> **Tip**
> Authentication with a password that has a short lifespan will be a TOTP.