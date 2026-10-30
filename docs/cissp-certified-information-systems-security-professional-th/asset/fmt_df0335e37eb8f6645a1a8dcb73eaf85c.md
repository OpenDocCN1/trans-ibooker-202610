
*   Validate or sanitize the data received from the server
*   Prevent eavesdropping of data exchanged with the server
*   Detect tampering with data exchanged with the server
*   Validate commands or code received from the server before executing or taking action based on information received from the server

To address these vulnerabilities, one can consider the following:

*   Evaluate your operating systems and applications for unpatched software or insecure configurations
*   Using a recognized secure protocol (e.g., transport layer security (TLS)) to validate the identity of the server and to prevent eavesdropping of, and tampering with, data communicated with the server
*   Using appropriate coding techniques to ensure that the data or commands received from the server are valid and consistent
*   Using digital signing to verify executable code received from the server prior to execution

In many cases, the client may use software libraries, applets, or applications to process data received from the server. For example, the client may rely upon image display components to permit the viewing of files. These components (e.g., Flash, Java Development Kit, etc.) will typically be provided by third parties and, as such, will need to be part of a vulnerability management program so that vulnerabilities that are discovered later can be patched in a timely manner.

If the client is a browser, then the browser ought to be configured in accordance with hardening guidelines available for the major web browsers. Similarly, the appropriate steps to protect and patch the underlying system that runs the client software need to be taken. Excellent sources of such guidance for browsers and client operating systems include The Center for Internet Security (CIS) and the Defense Information Systems Agency's Security Technical Implementation Guides.

The client system also needs to be protected from other threats as appropriate, based on the risk assessment and threat modeling. This could include firewalls, physical security controls, full-disk encryption, and so on.

If the software has been developed specifically for this application, then the appropriate secure software development process as described in [Chapter 8](#c08.xhtml) must be employed.

## Server-Based Systems

Server vulnerabilities mirror many of the same vulnerabilities described earlier, just from the server's perspective. It's important to remember that client and server systems are usually similar; the terms *client* and *server* only indicate the role a system plays in the overall system operations.

The server needs to validate the identity of the client and/or the identity of the user of the client. This can be done using a combination of Identity and Access Management (IAM) techniques along with a secure communications protocol such as TLS, using client-side certificates. TLS will also protect the server from eavesdropping and tampering, such as might happen from a MITM attack. The server also must validate all inputs and not assume that simply because the commands and data coming from the client are originating from (and have been validated by) the corresponding client-side software, they are valid and have been sanitized. The client must be considered untrusted, and it must be assumed that the client-end can insert or modify commands or data before being encrypted and transmitted over the secure (e.g., TLS) link. Depending on the nature of the environment, it might be appropriate to protect the server from DoS attacks by using techniques such as rate-limiting, CAPTCHA, and other approaches.

Certainly, a vulnerability management program is needed to ensure that updates and patches are applied in a timely fashion. This holds true regardless of whether the server-side software is developed in-house or is based in part or completely on software obtained from a third party (such as commercial off-the-shelf software, or COTS).

If the software has been developed specifically for this application, then the appropriate secure software development process must be employed. This includes ensuring that the server application runs only with the minimum permissions necessary, commonly understood as the “principle of least privilege.” If and when privilege escalation is needed, the period during which the server operates with elevated privileges is minimized. Best practices include the server using filesystem ownership and permissions to avoid data leakage, logging and monitoring appropriate information (such as successful and failed login attempts, privileged access, etc.), and capturing forensic information to permit analysis of a possible or actual security incident.

Finally, threats to the server itself need to be addressed. This may include physical and environmental threats, threats to the communications infrastructure, and server hardening as per industry recommendations such as those promulgated by the CIS (`www.cisecurity.org/cis-benchmarks`) or collected and indexed in NIST's National Checklist Program Repository (`nvd.nist.gov/ncp/repository`).

## Database Systems

Securing database systems is a special case of the more general server-based system security discussed in the previous section. If the database is accessible over a network, then all the security controls discussed there apply as well as those outlined below. If the database is not network accessible, then there are fewer risks, and some server security controls may not be necessary. Database-specific security controls include the following:

*   Consult the CIS's hardening guidelines for the database system being used. These guidelines include several of the recommendations below, and many others.
*   Only install or enable those components of the database system that are needed for your application.
*   Place data stores and log files on nonsystem partitions.
*   Set appropriate filesystem permissions on database directories, data stores, logs, and certificate files.
*   Run database services using a dedicated unprivileged account on a dedicated server.
*   Disable command history.
*   Do not use environment variables, command line, or database configuration files to pass authentication credentials.
*   Do not reuse database account names across different applications.
*   Disable “anonymous” accounts (if supported by the database).
*   Mandate that all connections use TLS if access or replication traffic travels over untrusted networks.
*   Use unique certificates for each database instance.
*   Use restricted views.
*   Ensure that all DBMS vendor-provided sample or test databases are removed or not accessible from user endpoints and clients.
*   Change all default passwords, ensure all accounts have secure passwords, and consider enabling multifactor or certificate-based authentication (where supported).
*   Ensure user account permissions have been assigned using the principle of least privilege and in alignment with enterprise-wide access control policies and procedures. Database privileges can be complex and interact in unexpected ways—avoid default roles and define those you need with only the permissions needed for each.
*   Disable or remove unneeded accounts, especially those with administrative permissions.
*   Manage all accounts according to best practices (see [Chapter 5](#c05.xhtml)).
*   Enable logging of sensitive operations and route logs to your log monitoring and alerting system.
*   Use `bind` variables where possible to minimize injection attack surfaces.
