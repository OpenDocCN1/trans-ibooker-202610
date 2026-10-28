marks because it’s one argument being passed to -c.

+   d. Once again, relock the root account while preserving the password:
+       sudo passwd -l root

+   e. This command should fail now:
+       su -c 'cat/etc/shadow'

+   f. This trick enables you to su into root even if the root password is not configured and/or the root account is locked:
+       sudo su

With sudo and your password, you are escalating your privileges to root, so the su for the root account works. By putting in your password in the sudo prompt, you’ve escalated your privileges, and they’re in place when the su is reached. Therefore, you’re not prompted for the root password, which may or may not exist.

Return to the context of your original user:
    exit

3a–3d

# Step 3 The sudo command is a much better choice than directly using the root account for many reasons.

The su command (which includes su, su -, su root, and su - root) enables multiple people that know the root password to share the root password. In the process, you don’t know who is acting as root for any command executed. You may lose track of who even has knowledge of the root password, which is terrible for security.

Using sudo, you authenticate with your own password, you are logged by your account, and you are held accountable when warranted. The log of every user that ran sudo and the commands they used sudo for can even help you remember what you typed in the past. sudo allows for granular control, allowing certain users to execute certain commands, as opposed to all commands, as is the case with su root. Furthermore, if the account of a member of the sudo group is in the wrong hands, the entire system isn’t in danger, but rather, just what that user can sudo with.