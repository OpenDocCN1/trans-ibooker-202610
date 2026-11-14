
# Security Vulnerabilities and Secure Coding Practices

Developers can prevent exploitation due to XML external entities by applying the following practices:

- Use SOAP 1.2 or higher
- Disable DTD and XML external entity processing on all XML parsers
- Ensure XML parsers are patched with the latest upgrades
- Use XML validation

## Broken Access Control

Applications should enforce what operations authenticated users are permitted to do, but many applications do not. This can lead to attackers being able to access users’ accounts, access sensitive data, or perform unauthorized functions. Applications are vulnerable if they contain any of the following weaknesses:

- Bypass the access control checks of other software
- Permit the elevation of privileges
- Permit cross-account access
- Permit manipulation of metadata, such as modifying tokens, cookies, or hidden fields
- Permit cross-origin resource sharing (CORS), which allows restricted web resources to be used from another domain

Developers can avoid broken access controls by applying the following practices:

- Deny by default
- Enforce access controls in accordance with the organization’s policies
- Avoid using CORS
- Disable web server directory listing

## Security Misconfiguration

Misconfigured software and operating systems are certainly the most commonly encountered vulnerabilities. Operating systems and software are often incorrectly configured due to the use of default settings, incorrect security settings, unneeded features, and even being out of date or unpatched. Applications are vulnerable if they contain any of the following weaknesses:

- Out-of-date or unpatched software
- Default accounts enabled
- Unneeded functions enabled
- Security features not enabled
- Security settings or values incorrectly set
- Overly verbose error messages (reveal more information than necessary)

Developers can prevent vulnerabilities due to misconfiguration by applying the following practices:

- Keep all software up to date with the latest versions and security patches
- Harden all software following configuration guidelines such as US DoD Defense Information Systems Agency (DISA) Security Technical Implementation Guides (STIGs) or Center for Internet Security (CIS) Benchmarks
- Enable only those features and settings that are absolutely necessary
- Review and apply security settings recommended by the product developer

## Cross-Site Scripting

Cross-site scripting (XSS) is a flaw that allows attackers to embed untrusted data in a web page. The data contains scripts that are executed by a victim’s browser, which can compromise systems and data. Victims are lured or redirected to the infected or malicious web page using social engineering techniques. Applications are vulnerable if they contain any of the following weaknesses:

- Lack of filtering of website input data (for example, allowing any content to be posted on a blog or social media site without checking it for malicious code)
- Applications or APIs that dynamically include attacker-controlled data

Developers can prevent XSS by applying the following practices:

- Use frameworks specifically designed to prevent XSS (such as Ruby on Rails or React JS)
- Educate users to not select links to untrusted web pages
- Disallow untrusted HTTP requests
- Apply content-sensitive filtering to browser APIs

## Insecure Deserialization

Serialization is the process of converting an object to bytes of data; deserialization is the reverse. If an attacker enters specially crafted data into a web page field of a vulnerable application, when the object is deserialized it can cause any number of exploits, including remote code execution, SQL injection, or other malicious unauthorized functions. Applications are vulnerable if they accept serialized objects from untrusted sources.

Developers can prevent insecure deserialization vulnerabilities by applying the following practices:

- Do not accept serialized objects from untrusted sources
- Perform integrity checking of serialized objects
- Run deserialization functions on separate isolated systems
- Restrict network traffic from systems that deserialize
- Monitor deserialization operation

## Using Components with Known Vulnerabilities

Components libraries used by applications can contain vulnerabilities and should be validated or sanitized before use, just like any software deployed to the environment. Applications are vulnerable if they contain any of the following weaknesses:

- Components of unknown origin
- Unknown versions of components
- Out-of-date or unsupported components
- Untested components

Developers can prevent exposure by applying the following practices:

- Implement an ongoing program to inventory, upgrade, and patch all component libraries
- Use components libraries only from trusted sources
- Obtain components from trusted sites and over secure links

## Insufficient Logging and Monitoring

Without sufficient logging, monitoring incidents may go undetected and be difficult to investigate and resolve. This can make a bad situation worse by delaying or complicating response and remediation actions. Applications are vulnerable if they contain any of the following weaknesses:

- No logging of errors and warning messages
- No logging of audit-worthy events such as failed logins
- Data is logged in format unsuitable for supporting investigation and understanding
- Local-only storage of audit logs
- Logging thresholds set too low or too high
- Incident detection thresholds not well-defined or aligned with logging

Developers can prevent logging shortcomings by applying the following practices:

- Tune logging thresholds based on the organization’s appetite and goals for risk
- Ensure data is logged in usable formats
- Log high-value transactions
- Implement a well-defined and well-rehearsed incident response capability

## Secure Coding Practices

Every organization that develops software should have a program in place to integrate information security into its SDLC. The program, whether formal or informal, should include `secure coding practices`, which are specific methods for developing software that does not contain security vulnerabilities. OWASP publishes guidance for developers, and the authors have found OWASP to be the most widely used industry guidance for this topic. OWASP provides secure coding guidance for developers to follow in the following categories:

- Input validation
- Output encoding
- Authentication and password management
- Session management
- Access control
- Cryptographic practices
- Error handling and logging
- Data protection
- System configuration
- Database security
- File management
- Memory management
- General coding practices

Many organizations follow OWASP guidelines or augment them with their own adaptation or specific implementation of coding practices based on their own environment and experiences. Secure coding practices should be documented so the organization has a consistent approach that can be applied, measured, and improved over time. Secure coding practices are best implemented as part of an overall program that includes the following:

- Incorporating security coding into SDLC, as discussed previously.
- Incorporating security into the supply chain to ensure software products acquired from COTS vendors or third-party developers do not contain security vulnerabilities.
- Establishing coding and development standards and libraries of reusable code.
- Continuously training developers
