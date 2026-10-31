# Other XSS attack vectors

Form parameters sent by the POST or GET methods are not the only ones used for XSS attacks. Header values such as User-Agent, Cookie, Host, and any other header whose information is reflected to the client are also vulnerable and susceptible to XSS attacks, even through the OPTIONS or TRACE methods. As penetration testers, you need to test completely all components of the request that are processed by the server and reflected back to the user.

# Exploiting Cross-Site Scripting

Hackers have been very creative when exploiting the XSS flaw, and with the capabilities of JavaScript in current browsers, the attack possibilities have increased. XSS combined with JavaScript can be used for the following types of attacks:

+   - Account hijacking
+   - Altering contents
+   - Defacing websites
+   - Running a port scan from the victim's machine
+   - Logging key strokes and monitoring a user's activity
+   - Stealing browser information
+   - Exploiting browser vulnerabilities

There are many different ways of triggering an XSS vulnerability, not only the <script></script> tag. Refer to OWASP's cheat sheet at the following link:
https://www.owasp.org/index.php/XSS_Filter_Evasion_Cheat_Sheet

In the following sections, we will look at some practical examples.

# Cookie stealing

One of the immediate implications of an XSS vulnerability is the possibility of an attacker using script code to steal a valid session cookie and use it to hijack a user's session if the cookie's parameters are not well configured.