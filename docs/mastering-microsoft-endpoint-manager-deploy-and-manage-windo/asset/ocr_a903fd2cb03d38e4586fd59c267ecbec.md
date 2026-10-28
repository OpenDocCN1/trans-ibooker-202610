Let's talk about the YubiKey. The end user experience looks very similar to how you normally log on to Windows. While you are normally logging on with either Windows Hello or your password, you can now select a USB key, as shown:

![](bbox=[0.233, 0.16, 0.732, 0.313])

Figure 13.32 – Windows sign-in options

After that, Windows will ask you to inject the security key into the USB port that holds the passphrase to log on to your Windows 10 device without a password.

![](bbox=[0.263, 0.4, 0.703, 0.69])

Figure 13.33 – FIDO2 authentication

First, you need to enable password-less authentication in your AAD tenant.

# Enabling password-less authentication

To enable password-less authentication, you have to go to the Azure portal and open AAD. Then, follow these steps:

+   1. Go to Security.
+   2. Open Authentication methods.