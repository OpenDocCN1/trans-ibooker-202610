| Time | User | Event | Password |
| 09:08:23 | fred | Login: Failure | password |
| 09:08:24 | ian | Login: Failure | password |
| 09:08:25 | john | Login: Failure | password |
| 09:08:27 | carol | Login: Failure | password |
| 09:08:28 | mary | Login: Failure | password |
| 09:08:23 | fred | Login: Failure | letmein |
| 09:08:24 | ian | Login: Failure | letmein |
| 09:08:25 | john | Login: Failure | letmein |
| 09:08:27 | carol | Login: Failure | letmein |
| 09:08:28 | mary | Login: Failure | letmein |

Figure 9.5 – Password Spraying

+   - **Plaintext/Unencrypted**: These passwords can be cracked by a brute-force attack.
+   - **Directory Traversal**. This is where an attacker navigates through the structure of a web server moving from page to page searching for login details. It is known to use dot-dot-slash (../) to navigate through the directory structure, bypassing the webserver's root. The formats ..2f and %2e%2e%2f represent ../.
+     Example 1: The URL for the webserver will include ../../../../etc/passwd – this file lists the system accounts information including the user's ID and it is plain text. It should have read-only permissions.
+     Example 2: The URL for the webserver will include ../../../../etc/shadow – this file lists the passwords of the users, it is an encrypted file.
+     This is also known as path traversal.
+   - **Account Lockout**: Account lockout refers to the number of attempts a user can make to insert their password before being locked out of the system. If I set the account lockout at three attempts, it will stop both dictionary and brute-force attacks after three attempts.
+   - **Minimum Number of Characters**: Should account lockout not be an option, the best way to prevent the password from being cracked is by salting or key stretching, where randomized characters are appended to a password. This will prevent a dictionary attack, but will only slow down a brute-force attack.

Tip
Setting account lockout at a low value will prevent a brute-force attack.