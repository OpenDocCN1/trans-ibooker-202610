| Tool | Pro's | Con's |
|---|---|---|
| **SAST (Static Application Security Testing)** | - Can find line number where issue exists
- Applied early in the lifecycle
- Can make remediation recommendations | - Tool may not support all languages in the organization.
- Can produce many false positives.
- Does not see the code while it's running. |
| **DAST (Dynamic Application Security Testing)** | - Not language or technology dependent
- Not as many false positives as SAST
- Can test software you don't own | - Can't always locate the impacted line of code
- Can't find code specific issues
- Found later in the SDLC |
| **IAST (Interactive Application Security Testing)** | - Agents or library that is continuously monitoring
- Lower cases of false positives
- Works well in DevSecOps | - More difficult to deploy and maintain.
- Potential development work in order to integrate it
- Can usually only see something as it's tested |

As I previously mentioned, DAST can be run in a production environment, but this not recommended as it can have unforeseen impacts such as availability issues or corruption of data. There are options to have DAST run less destructive testing, but the organization needs to weigh the cost and benefit. Another alternative reason for running DAST in production would be for testing the security tooling and alerting for the security organization. Perhaps the organization would like to learn whether the security operations center (SOC) is able to detect incoming attacks. Otherwise, so long as the organization has confidence that its pre-production environment mirrors its production environment (and it really should) then running DAST in the pre-production environment should suffice.

> **NOTE** In reality, most organizations will only choose one lower environment to run these types of tools. License cost can quickly explode if you are being charged "per environment". It depends on the vendor and their license structure.

# 4.3.4 SCA in DevSecOps

As I covered in previous chapters, software composition analysis (SCA) is used to detect issues with libraries used to build the application that are from a source outside of the organization. These are typically libraries that are from third-party sources or within the frameworks used to build and run the application. An example would be log4j used by Apache as a logging framework. Frequently, these third-party libraries will be found to have vulnerabilities in their code that require patching. Nothing different than what any organization would do if they found a security issue within their own code that required a