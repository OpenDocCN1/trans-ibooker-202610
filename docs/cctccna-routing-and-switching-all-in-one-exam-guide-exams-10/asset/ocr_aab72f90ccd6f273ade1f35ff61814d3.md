device (if telnet was configured and you had the password). This is how you can document the network topology using CDP.

# Licensing Cisco Devices

Licensing Cisco ISR G2 routers has become more and more common across many of Cisco platforms; even Cisco’s switches are starting to support licensing of features and use. Prior to Cisco IOS version 15 on Cisco routers, Cisco had up to 12 different image types for a particular device model and software version. To simplify this, Cisco now has only four image types:

+   - IP Base
+   - Data (MPLS and ATM)
+   - Unified Communications (VoIP and IP telephony)
+   - Security (firewall, IPS, and VPNs)

You can use the **show license feature** command to view the technology package licenses and the feature licenses supported by your router. Here’s an example of the use of this command:

```
Router# show license feature
Feature name  Enforcement Evaluation Subscription Enabled RightToUse
ipbasek9      no          no          no           yes      no
securityk9    yes         yes         no           yes      yes
uck9          yes         yes         no           yes      yes
datak9        yes         yes         no           no       yes
LI            yes         no          no           no       no
ios-ips-update yes        yes         yes          no       yes
```

Licensing is enforced starting with Cisco ISR G2 routers (1900, 2900, and 3900 series). Licensing unlocks features in IOS code. Licenses come in two types:

+   - **Evaluation** This license is a temporary license that is valid for a particular period of time, such as 60 days or one year.
+   - **Permanent** This license is valid for the life of the product.