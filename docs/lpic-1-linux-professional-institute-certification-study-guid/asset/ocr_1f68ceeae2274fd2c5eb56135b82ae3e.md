# # Review Questions

You can find the answers in the appendix.

+   1. Which of the following are fields within a /etc/passwd file record? (Choose all that apply.)
+   A. User account's username
+   B. Password
+   C. Password change date
+   D. Special flag
+   E. UID

+   2. Which of the following are fields within an /etc/shadow file record? (Choose all that apply.)
+   A. Maximum password age
+   B. Account expiration date
+   C. Password
+   D. Comment
+   E. Default shell

+   3. Which of the following commands will allow you to view the NUhura account's record data in the /etc/passwd file? (Choose all that apply.)
+   A. getent NUhura passwd
+   B. cat /etc/passwd
+   C. passwd NUhura
+   D. grep NUhura /etc/passwd
+   E. getent passwd NUhura

+   4. You create an account using the useradd utility, except for some reason the account's home directory was not created. Which of the following most likely caused this to occur?
+   A. The HOME directive is set to no.
+   B. You did not employ super user privileges.
+   C. The CREATE_HOME directive is not set.
+   D. The INACTIVE directive is set to -1.
+   E. The EXPIRE date is set and it is before today.

+   5. Which of the following commands will allow you to switch temporarily from your account's default group to another group with whom you have membership?
+   A. The usermod command
+   B. The newgrp command
+   C. The groups command
+   D. The groupadd command
+   E. The groupmod command