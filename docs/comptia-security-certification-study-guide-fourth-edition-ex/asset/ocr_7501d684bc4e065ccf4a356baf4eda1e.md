+   5. Type P@ssw0rd in the Password field and Confirm Password field and then clear the check box that states you will need to change the password at next logon. Click Next and then Finish.
+   6. To create a security group, right-click the Users folder and choose New | Group.
+   7. Type Authors as the name of the group and then click OK.
+   8. To assign permissions to the group, go to File Explorer and navigate to drive C:. Right-click drive C:, choose New | Folder, and create a folder on drive C: called Publications.
+   9. To assign permissions to the Authors group, right-click the Publications folder and choose Properties. Click the Security tab.
+   10. To add the authors to the permission list, click Edit and then click Add.
+   11. Type authors in the Select Users, Computers, or Groups dialog box. Click OK.
+   12. Select Authors in the permission list and then assign the group the Modify permission.
+   13. Click OK twice.

# Rights and Privileges

> exam Watch The Security+ exam expects you to understand proper usage of groups and permissions. Remember that the user is assigned to the group, and the group is assigned the permissions.

A big part of the security of a system comes from the fact that only certain security principals (users or groups) can perform certain actions. For example, it would be a huge security concern if anyone could do a backup of any files on a system. In the Microsoft world, only a person with the “Back up files and directories” right can perform backups.

On a Windows system, you normally have to be logged in as the administrator to perform changes to a system, while in Linux, you typically need to be logged in as root to be able to make changes. It is possible to allow others to make changes to the system by granting them the correct rights or privileges.

Microsoft operating systems have a User Rights Assignments section of system policies where you can control who can perform common administrative tasks such as backing up files and directories or changing the system time. The following outlines some common rights in Windows:

+   Access this computer from the network  This right controls who is allowed to connect to the system from across the network.
+   Allow log on locally  This right controls who is allowed to sit at the computer and log on.