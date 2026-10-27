The following command builds on the previous command by adding the user account to the local administrators group of the system. This means the hacker now has a back door with full administrative capabilities.

```
pass';exec master..xp_cmdshell
"net localgroup administrators fromSQL /add" --
```

exam Watch For the exam, know that to protect against an SQL injection attack, the developers of the application must validate the input before processing it.

There are a number of ways you can prevent SQL injection attacks:

+   - **Sanitization** Developers should clean any characters out of user input that could be executed as SQL code. For example, they should remove semicolons and double dashes from the input.
+   - **Validation** Developers should validate input and ensure they restrict the number of characters that can be inputted and the type of characters used.
+   - **Parameterized queries** Using parameterized queries means that the input is not passed to an SQL statement directly, but into a parameter that only accepts certain values. If the parameter value is good, then it is used by the SQL code within the parameterized query.

# EXERCISE 15-2

# SQL Injection Attacks

In this exercise, you see how hackers can bypass application security by performing an SQL injection attack into a web site that connects to SQL Server. To perform this exercise, be sure to have an SQL Server installed and a web site that connects to that SQL Server.

+   1. Ensure that you have a server running SQL Server and a web site that queries that SQL Server. Also ensure that the Windows 10 and ServerA VMs are running.
+   2. Go to the Windows 10 VM and start Internet Explorer. Type http://10.0.0.1 to navigate to the course site.
+   3. To bypass the logon screen with an SQL injection attack, type your first name in the username box, and then type pass' or 1=1 -- in the password box and log on. No username or password was really needed!