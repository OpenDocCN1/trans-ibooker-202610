# TWO-MINUTE DRILL

# Understanding Application Attacks

+   - An SQL injection attack involves the hacker inserting SQL statements into forms in an application to try to control what the application does.
+   - A buffer overflow attack is when the hacker sends too much data to an application, which typically results in the hacker obtaining shell access to the system with administrative permissions.
+   - A zero-day exploit is an exploit that is unknown to the vendor of the vulnerable application but is known to the hacker community.
+   - To prevent application attacks, developers must ensure validation of any input sent to the application before acting on that input. Network administrators can prevent application attacks by patching all software and operating systems on a regular basis.

# Secure Application Development Concepts

+   - Remember that an application should be deployed to the following environment in this order: Development → Test → Staging → Production → Quality Assurance (QA).
+   - Developers implement error handling within most programming environments by implementing **try/catch** blocks.
+   - Any data inputted into an application should be validated before being processed.
+   - Stored procedures should be used to help prevent against SQL injection attacks.
+   - In distributed applications, implement input validation on the client side and the server side (especially on the server side).

# Implement Host and Application Security

+   - You can control what applications are allowed to run on a system by using an allow list (formerly known as whitelisting).
+   - You can deny a specific application from running on a system by using a block/deny list (formerly known as blacklisting).
+   - Fuzzing is a way to test how an application responds to random input.