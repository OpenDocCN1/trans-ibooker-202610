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

# Dynamic-Link Library Injection

Dynamic-link library (DLL) injection occurs when a program is forced to load a DLL in its address space and run the code of the DLL. The code in the DLL could be malicious code the hacker wants to execute on the system.

# Lightweight Directory Access Protocol Injection

A Lightweight Directory Access Protocol (LDAP) injection is when the hacker fills out a web form that normally uses the form data to query a database with an LDAP call, but because the application developer has not validated any of the input, the hacker can insert whatever they want into the page, and the content will get executed by the server.

# Extensible Markup Language Injection

An Extensible Markup Language (XML) injection is similar to an SQL injection and an LDAP injection, except that the hacker is inserting XML code into the application. If the application developer has not validated the input, the hacker could manipulate the flow of the application with the XML data that is injected.

You can prevent all different types of injection attacks by sanitizing input and validating input.

# Buffer Overflow Attacks

Most attacks are buffer overflows. A buffer is an area of memory used to store information sent to an application. A buffer overflow is when a hacker sends too much information to the application, causing the information to fill both the buffer and memory outside the buffer (see Figure 15-3).