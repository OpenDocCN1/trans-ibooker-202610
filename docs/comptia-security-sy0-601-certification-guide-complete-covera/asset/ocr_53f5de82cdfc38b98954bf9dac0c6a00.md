The highest would be 777 as the owner, group, and other all have full control. The lowest would 644 as the owner would have read and write, the group would be read, and the other would also be read.

Another way the permissions can be set is by alphabetical values, as shown:

+   a. R: Read
+   b. W: Write
+   c. X: Execute

When using alphabetical values, each set of permissions is shown as three dashes.

Full control for the three entities is as follows:

+   - Owner full control would be shown as rwx --- ---.
+   - Group full control --- rwx ---.
+   - User full control --- --- rwx.

You can also change permissions in Linux. If the permission to File C is 654 and we wish to change these permissions to give full control, we will run the chmod 777 File C command, which changes the permissions to File C.

# Role-Based Access Control

This is a subset of duties within a department. An example would be two people within the finance department who only handle the petty cash. In IT terms, it could be that only two of the IT team administer the email server.

# Rule-Based Access Control

In Rule-Based Access Control (RBAC), a rule is applied to all of the people within a department, for example, contractors will only have access between 8 a.m. and 5 p.m., and the help desk people will only be able to access Building 1, where their place of work is. It can be time-based or have some sort of restriction, but it applies to the whole department.

# Attribute-Based Access Control

In Attribute-Based Access Control (ABAC), access is restricted based on an attribute in the account. John could be an executive and some data could be restricted to only those with the executive attribute. This is a user attribute from the directory services, such as a department or a location. You may wish to give different levels of control to different departments.