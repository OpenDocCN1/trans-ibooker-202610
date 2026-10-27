9. Your manager has heard about this new game that is being sent around through e-mail that everyone is downloading and playing. There is rumor that the game, called newgame.exe, may be a Trojan virus. What could you do to prevent users from running the game on company systems?

+   A. Use an allow list.
+   B. Use a block list.
+   C. Use a stored procedure.
+   D. Digitally sign the app.

# Performance-Based Question

10. Match the item on the left to the best corresponding item on the right.

+   Synchronization token
+   pass' or 1=1;update titles set price=.5 -
+   http://site/processpayment.asp?txtName=<script>alert('hello')</script>
+   Escape characters
+   http://10.0.0.101/scripts/../%c0%af../winnt/system32/cmd.exe/?c=dir+c:\
+   Input validation

+   Prevent injection attacks
+   Directory traversal attack
+   A SQL injection attack
+   Used to prevent CSRF attacks
+   Used to prevent XSS attacks
+   A XSS attack

# SELF TEST ANSWERS

# Understanding Application Attacks

1. ☑ B. When you see a packet with pass' or 1=1--, you are most likely being attacked with an SQL injection attack. An SQL injection attack is when the hacker inserts SQL commands into an application to control the flow of the application.

☒ A, C, and D are incorrect. A buffer overflow attack is when the hacker sends too much data to an application, causing the data to be stored beyond the buffer area. Directory traversal is when the hacker places ../.. in the URL to try to navigate out of the web site folder and access information on the web server. Cross-site scripting is when the hacker finds a way to send client-side script to the server so that it is processed by a client, such as submitting script into a field in a web site, which is then stored as data in the web site database. When a user visiting the site displays the data, the code executes on the client system.