# Installing a CA

If you want to create your own certificates for internal use within the business, you need to install a CA. To install a CA on a Windows server, follow the steps in the following exercise.

# EXERCISE 13-1

# Installing a Certificate Authority

In this exercise, you install the Active Directory Certificate Services (ADCS) on a Windows Server in order to create a certificate authority.

+   1. Launch Server Manager from the Start menu.
+   2. Within Server Manager, choose Manage | Add Roles and Features.
+   3. On the Before You Begin page, click Next.
+   4. On the Select Installation Type page, click Next to install a role or feature.
+   5. On the Select Destination Type page, notice that the ServerA.certworld.loc server is selected and then click Next.
+   6. On the Select Server Roles page, choose the check box for Active Directory Certificate Services to install it and then choose Next.
+   7. Choose Add Features when the window pops up and asks you to add required features.
+   8. Click Next three times. On the Select Role Services page, ensure that both the Certification Authority check box and the Certification Authority Web Enrollment check box are selected. Then click Next.
+   9. Click Install. The installation will take a few minutes, and then you can click Close.
+   10. Now that you have installed the certificate authority, you can perform the initial configuration by choosing the flag icon (to the left of Manage) and then choosing the link “Configure Active Directory Certificate Services on this server.” On the Credentials page, notice the administrator account is being used (at the bottom) and then click Next.
+   11. On the Role Services page, choose both the Certification Authority and Certification Authority Web Enrollment check boxes and then click Next.
+   12. You are then asked for the type of setup you wish to perform on the Setup Type page:

+   Enterprise CA  Used to create a root CA in an Active Directory environment
+   Standalone CA  Used to create a root CA on a server that is not in an Active Directory environment