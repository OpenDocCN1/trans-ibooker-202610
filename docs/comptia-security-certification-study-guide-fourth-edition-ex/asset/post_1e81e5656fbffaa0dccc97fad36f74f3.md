
4. Once a hacker has verified that they can bypass logon, then they may try to perform malicious acts against your data, such as changing the price of all the books:

```
pass' or 1=1;update titles set price=.5 --
```

5. Next, the attacker may want to call operating system commands through the web application using an SQL injection attack. Note that in order for this to work, you must enable xp_cmdshell support in SQL Server. In the following example, you create a user account named “fromSQL” and place that user in the administrators group using SQL injection:

```
pass';exec master..xp_cmdshell
"net user fromSQL password /add" --

pass';exec master..xp_cmdshell
"net localgroup administrators fromSQL /add" --
```

## Dynamic-Link Library Injection

Dynamic-link library (DLL) injection occurs when a program is forced to load a DLL in its address space and run the code of the DLL. The code in the DLL could be malicious code the hacker wants to execute on the system.

## Lightweight Directory Access Protocol Injection

A Lightweight Directory Access Protocol (LDAP) injection is when the hacker fills out a web form that normally uses the form data to query a database with an LDAP call, but because the application developer has not validated any of the input, the hacker can insert whatever they want into the page, and the content will get executed by the server.

## Extensible Markup Language Injection

An Extensible Markup Language (XML) injection is similar to an SQL injection and an LDAP injection, except that the hacker is inserting XML code into the application. If the application developer has not validated the input, the hacker could manipulate the flow of the application with the XML data that is injected.

You can prevent all different types of injection attacks by sanitizing input and validating input.

## Buffer Overflow Attacks

Most attacks are buffer overflows. A buffer is an area of memory used to store information sent to an application. A buffer overflow is when a hacker sends too much information to the application, causing the information to fill both the buffer and memory outside the buffer (see Figure 15-3).

### FIGURE 15-3

Looking at a buffer overflow attack

![](img/3e3af33e2c753e920c09ecd40b5a457a_658_0.png)

If the hacker can store information in memory beyond the buffer area, the hacker can run whatever code they want with administrative privileges. The software that is susceptible to this attack could be an application or a background service loaded in the operating system.

To see an example of a buffer overflow attack, check out the video included in the online resources that accompany this book.

## Cross-Site Scripting

Cross-site scripting (XSS) is a popular form of attack that involves the hacker inserting script code into a form on a web page and submitting the script code to the server. The idea here is not to attack the server but to have the server feed that script to another client, which parses the script and executes it. A simple example where cross-site scripting can occur is if the hacker submits the script code in a field that is stored in a database. When another person visits the site, the web server will return the data in the database and send it to the client browser—this code will then be parsed and executed by the client.

There are different types of cross-site scripting attacks:

- **Reflected** A reflected XSS attack is when the script code is inserted into the form and then submitted to the server to process. The server processes the script at that time instead of storing it in a database, as described earlier.
- **Stored/persistent** A stored XSS attack, also known as persistent XSS, is what was described earlier, where the hacker submits the script and the server stores it in the database in the background (such as a discussion form site). When someone visits the site, the site retrieves the data from the database and then sends the content in the HTML page to the user. The user's browser reads the HTML to create the page, and when the script code is encountered, it is executed on the client system.

The following is a typical example of what the URL might look like when an XSS attack is attempted by a hacker against your web site:

```
http://site/processpayment.asp?txtName=<script>alert('hello');</script>
```

## SSL Stripping

An SSL stripping attack is when the hacker is able to place themselves between the victim and a secure HTTPS site that the victim uses. When the user sends a request to the secure site, the hacker intercepts the request and essentially creates their own secure connection with the target web site. The communication between the victim and the hacker is downgraded to unsecure HTTP communication (allowing the hacker to view all of the data), but the victim traffic is then sent to the secure site by the hacker using HTTPS.

## Driver Manipulation and Refactoring

Following a continuous code-improvement mindset, we are always looking for better ways to improve our code. When an application has problems running in an environment, a shim can be created and applied to the application to fix compatibility issues. As it relates to driver manipulation, a shim can be created to allow a driver to run on an OS that the driver had compatibility issues with. From a security point of view, shimming an application could create a new vector for attacks because rewriting functions in a shim and applying the shim to the application or driver could introduce a flaw by accident—or on purpose if the person who created the shim is the hacker. Driver refactoring is when a stock driver is replaced with a driver that is malicious. When applications make calls to the refactored driver, they are executing the malicious code.

## Other Application Attacks

There are a number of other application attacks you need to be familiar with for the Security+ exam. The following identifies the additional types of application attacks:

- **Race conditions** A race condition is a software programming issue where code executed by a thread (a thread is a unit of work) must complete in a specific order before another thread can execute that same logic. For example, let’s say there are three steps to a deposit action in a bank account: (1) read the account balance, (2) add the amount being deposited, (3) save the new balance amount back to the account balance. If two threads or processes execute the logic, we would assume that each step is completed in order before the next thread executes the logic; otherwise, the account balance will be inaccurate. For example, let’s say there is $100 in the bank account. If the logic executes out of order (a race condition), then you could have thread 1 read the account balance ($100), and let’s say thread 2 also reads the account balance ($100). Then each thread adds $50 and saves the value back. In the end there will only be $150 in the balance instead of the desired $200. This is because a race condition occurred, where we need each thread to execute all three steps before the next thread or process can execute the same logic. This is known as a *time of check/time of use (TOCTOU)* software bug, and to fix this you should have the code check the object being accessed to ensure it is not already being accessed, and then minimize the time the object is used and release it again so other code can access it.
- **Replay attack** A replay attack, or session replay attack, is when the attacker intercepts traffic from the victim and is able to grab their session ID from that traffic. The attacker can then resubmit traffic to the target, essentially impersonating the victim because the attacker has the session ID.
- **Application programming interface (API) attacks** An API is a library of functions that a programmer creates that provides some form of functionality. An API attack is when a hacker tries to use that API for malicious purposes—typically by making calls to the functions and performing injection attacks on those functions.
- **Resource exhaustion** Insufficient manpower to get the job done properly is a type of vulnerability. For example, not having enough programmers may result in cutting corners and skipping important tasks such as input validation. Similarly, overworked programmers are more prone to making errors.
- **Memory/buffer vulnerability** There are a number of memory vulnerabilities or buffer vulnerabilities to be aware of:
  - **Memory leak** A memory leak occurs when a program does not release memory for other processes to use after it is done with it. If each program held on to memory after it was done with it, the system would run out of memory and crash.
  - **Integer overflow** When data is stored in memory, it is assigned a block of memory of a specific size. An integer overflow error occurs when a block of memory has been created to store an integer value, but the result of the arithmetic operation exceeds the maximum size of the integer type assigned. It has been found that the application has unexpected responses to dealing with integer overflows, thus presenting a security issue.
  - **Buffer overflow** A buffer overflow occurs when software writes to areas of memory outside the areas it has been assigned.
  - **Pointer/object dereference** Pointers are a tool programmers use to refer to an area of memory. When a programmer wants to read the contents of memory, they *dereference the pointer*. From a security point of view, make sure you block the execution of untrusted or injected code so that hackers cannot read these areas of memory.
