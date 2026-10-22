![](bbox=[0.18, 0.12, 0.83, 0.52])

# Figure 3.6 – FindSecBugs in Eclipse

Along with showing the line of code that has the vulnerability, FindSecBugs will provide reasons why it is vulnerable and possible solutions. There is a myriad of other similar tools, and as mentioned, most vendors with security solutions, especially in the SAST and SCA space have developer tools.

The goal here is to provide the developer the resources to resolve a potential vulnerability prior to them checking the code in to the code branch. Other tools in the pipeline, like the ones I outlined previously, are still needed however having the ability to catch an issue before it goes to a code branch means that there is less reliance on these tools to find vulnerabilities when the code has already moved out of the developer IDE, and may reduce the amount of effort spent in these other tools to find and resolve issues.

# 3.2.3 Dynamic application security testing

Where SAST and IDE integration tools are looking at the code in its static form, dynamic application security testing (DAST) will scan the application as it is running in an environment. It can be thought of as a penetration test that is performed by a tool considering that it is a security test that is looking from the outside in and attempts to locate weaknesses such as parameter tampering, cross-site scripting, improper redirects, and so forth. DAST tools are often used by penetration testers to identify low-hanging fruit in a running application and take some of the manual work out of a penetration test.