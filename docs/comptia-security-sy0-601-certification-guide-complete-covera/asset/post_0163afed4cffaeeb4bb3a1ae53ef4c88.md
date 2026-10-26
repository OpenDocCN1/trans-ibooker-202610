
47. What is the relationship between the RPO and the RTO?
48. What information can be established from an MTTR?
49. What type of threat actor could damage a company’s production system?
50. What type of threat actor would demand payment from you or threaten to publish customer information that you hold on social media?
51. What is the purpose of MTBF?
52. What is the purpose of SSAE?
53. What is the purpose of SLE and how is it calculated?
54. How can we calculate the ALE?

## 11 Managing Application Security

One of the main tasks that the cybersecurity team deals with is the management of applications used within the company, to ensure that they have no vulnerabilities and have been hardened. In this chapter, you will learn to deploy and manage applications effectively and select the appropriate development life cycle. You will learn about secure coding techniques, quality control, and testing. Let’s begin by looking at implementing hosts and ensuring that they are secure before rolling them out.

In this chapter, we will cover the following topics:

- Implementing Host or Application Security
- Understanding the Security Implications of Embedded and Specialist Systems
- Understanding Secure Application Development, Deployment, and Automation

### Endpoint Protection

It is important that the endpoints are protected from an attack. Let's look at each of the endpoint protections, in turn, starting with anti-virus:

- **Anti-Virus**: Anti-Virus monitors websites that are being visited and the files that are being downloaded to ensure that they are not affected by viruses or trojans. Most modern anti-virus solutions can carry out the role of anti-malware.
- **Anti-Malware**: Anti-malware scans your computer for *adware* and *spyware*, and prevents malicious software from running.
- **Endpoint Detection and Response (EDR)**: An EDR is an advanced solution that is better than anti-virus or anti-malware. It is a centralized console that continuously monitors the computer, and makes automatic alerts when a threat has been detected. It uses machine learning to detect threats and can detect file-less viruses.
- **Data Loss Prevention (DLP)**: An endpoint DLP solution can be set up so that it can protect data on your computer from being stolen by using email or a USB drive. DLP can also protect any data that has a pattern match, such as PII information or sensitive data.

  **Example:** I have a file called *new business contacts* and I want to ensure that it cannot be stolen. It does not have a pattern match. However, I can use the DLP solution, select the file, and a pattern match will be assigned to the file. When someone tries to email the file, it will be blocked by the DLP system.

- **Next-Generation Firewall (NGFW)**: An NGFW is more than a traditional firewall. It can act as a stateful firewall by carrying out deep packet filtering. It can also inspect application traffic to ensure that it is legitimate and use whitelisting to ensure that only approved applications are allowed to run. It can also act as an intrusion prevention device protecting against an attack, and it can inspect encrypted SSL and HTTPS packets.
- **Host Intrusion Prevention System (HIPS)**: A HIPS is a software program that can be installed on a host to protect it against attack. It analyzes the behavior of a computer and looks for any suspicious behavior in log files and memory and takes the appropriate action to prevent attacks such as malware.
- **Host Intrusion Detection System (HIDS)**: A HIDS, by its very nature, is a passive device that monitors patterns in the behavior of a computer system. The HIDS uses a database that contains the settings for the computer, including the registry, critical system files, applications, and components. Its function is to alert the user to any discrepancies or attacks.
- **Host-Based Firewall:** A host-based firewall can be used to prevent unauthorized access to the desktop and can set up permitted rules for approved applications.

![](img/adfe46a7d545f23d50e4108d31e60de2_362_0.png)

Figure 11.1 – Host-based firewall

### Databases

It is very important that we keep databases safe as they may contain PII information about individuals or credit card information. Let’s look at what we can do to keep our database as secure as possible, starting with stored procedures:

- **Stored Procedures:** A SQL injection attack manipulates a SQL statement and then ends it by inserting 1=1. Instead of writing Transact-SQL statements, we can have the SQL script prewritten and saved in a stored procedure (for example, ABC). This prevents manipulation of the statement. We then insert ABC into the Transact-SQL, thereby preventing alteration. This is similar to what happens with a batch file. Another method of detecting a SQL Injection is checking for an apostrophe in a data field.
- **Tokenization:** Tokenization is deemed more secure than encryption. When we encrypt data, we use a public key, and then we can reverse the encryption by decrypting the data with the private key or by cracking the encryption. This means that an attacker could decipher the data. Tokenization takes sensitive data such as a credit card number and replaces it with random data. Therefore, it cannot be reversed. Refer to *Figure 11.2* to understand the process of tokenization:
- **Salting:** Salting passwords in a database means that we take the password stored in the database and add randomized numbers to increase the compute time for a brute-force attack.

### Application Security

In today's world, we are all interconnected and use an ever-increasing number of applications that can be targeted by attackers. Therefore, as a cybersecurity professional, we need to ensure that we harden these applications to prevent attacks. Let's look at the types of protection that we can put in place, starting with input validation:

- **Input Validation:** Controlling inputs to an application is vital to ensure that *buffer overflow*, *integer overflow*, and *SQL injection* attacks cannot be launched against applications and databases. Input validation is where data is entered either using a web page or wizard. Both are set up to only accept data in the correct format within a range of minimum and maximum values.

  **Example:** Have you ever completed a web form quickly and maybe put your zip code into another field? This results in an error in the form and it fails to process the submit button. The webform then has a list at the top, in red, of the incorrect parameters, with a red star next to each of them. Once you have corrected the entries, the form will accept and submit them.

- **Secure Cookies:** Cookies are used by web browsers and contain information about your session, and they can be stolen by attackers to carry out a session hijacking attack. We can set the secure flag on the website to ensure that cookies are only downloaded when there is a secure HTTPS session. You can do this in Java EE 6 by applying the following information to the web.xml file. An example is shown in the following code snippet:

  ```
  <session-config>
  <cookie-config>
  <secure> 'true' </secure>
  </cookie-config>
  </session-config>
  ```

- **Hypertext Transfer Protocol (HTTP) Headers**: HTTP headers are designed to transfer information between the host and the web server. An attacker can carry out *cross-site scripting* as it is mainly delivered through injecting HTTP response headers. This can be prevented by entering the **HTTP Strict Transport Security (HSTS)** header:

  This means that HSTS ensures that the browser will ignore all HTTP connections.

- **Code Signing**: A code-signing certificate is procured that allows you to digitally sign scripts and executables to verify their authenticity and to confirm that they are genuine.
- **Allow List**: An allow list only allows explicitly allowed applications to run. This can be done by setting up an application whitelist. Firewalls such as pfSense can have an allow list.
- **Block List/Deny List**: A block/deny list prevents explicitly blocked applications from being set up by using a blacklist to prevent banned applications from running. Firewalls such as pfSense can have a block list.
- **Secure Coding Practices**: This is where the developer that creates software ensures that there are no bugs or flaws so that they can prevent attacks such as *buffer overflow* or *integer overflow*.
- **Static Code Analyzers**: When developers use static code analyzers, the code is not executed locally. Instead, they launch the static code analyzer tool. Then the source code is run inside the tool that reports any flaws or weaknesses.
- **Dynamic Code Analysis**: When developers use dynamic analysis, the code is run locally, and then they use a technique called *fuzzing*, where a random input is inserted into the application to see what the output will be. White box pen testers use fuzzing to see the flaws and weaknesses in an application before it is rolled out to the production environment.
- **Manual Code Review**: This is where a developer reads code line by line to ensure that the code is written properly and that there are no errors. This is very tedious and time-consuming.

```
function requestHandler(req, res) {
  res.setHeader('Strict-Transport-Security','max-age=31536000; includeSubDomains; preload');
}
```
