Click the Start button or in the search box, type **PowerShell**, and then select Windows PowerShell. At the prompt, type **whoami** and press ENTER. The output will show your domain, a backslash, and the name of the user you logged in as.

6a, 6b

**Step 6** As you learned in Chapter 11, passwords should be stored in hashed format. If a user forgets their password and calls up the help desk or a systems administrator and says “I forgot my password. Can you tell me what it is?”, the honest answer is “No.” A better request would be “Can you reset my password?”, to which the answer should be a resounding “Yes!”

That’s what you’ll do now.

+   a. On the Windows Server 2019 VM, in Active Directory Users and Computers, right-click the non-admin user account and select Reset Password. Enter and confirm a password. Leave the check in the box next to User Must Change Password at Next Logon.
+   b. Sign in with the new password and change it when prompted.

7g

**Step 7** When a user account needs to be removed from AD because a user quits or is fired, it is a better idea to disable the account than to delete it. The reason is for forensics analysis, if necessary. There might be evidence that you don’t know you’ll need down the road, or there might actually be an active investigation. You can even put disabled accounts into a special OU created for that very purpose. You’ll disable and enable an account now.

+   a. From the Windows 10 VM, sign out of the domain with the non-admin user.
+   b. From the Windows Server 2019 VM, right-click the non-admin user account and select Disable Account.
+   c. Right-click it to see that now it shows Enable Account. Don’t click it, though.
+   d. The disabling and enabling of an account can also be done by right-