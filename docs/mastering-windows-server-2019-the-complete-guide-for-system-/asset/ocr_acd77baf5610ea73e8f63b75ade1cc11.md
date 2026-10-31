Even though this technology is now being morphed into something at the cloud level, let's take a few minutes and review a couple of screenshots from the ATA interface so you have an idea of what this social media-style feed looks like. This screenshot was taken from a Microsoft demo where they purposefully stole the Kerberos ticket from a user, and then utilized it on another computer in order to access some confidential files that only Demi Albuz should have been able to access. While ATA did not stop this activity, it immediately—and I mean within seconds—alerted inside this feed to show the Pass-the-Ticket Attack:

![](bbox=[0.12, 0.25, 0.87, 0.6])

# Identity Theft Using Pass-the-Ticket Attack

Demi Albuz's Kerberos tickets were stolen from CLIENT1 to CLIENT2 and used to access DC1 (CIFS).

Recommendations

+   - Disconnect the relevant computers from the network or move them into an isolated environment and start a forensics procedure by investigating: unknown processes, services, registry entries, unsigned files, and more
+   - Disable Demi Albuz's account

Figure 9.30: ATA identifying a pass-the-ticket attack