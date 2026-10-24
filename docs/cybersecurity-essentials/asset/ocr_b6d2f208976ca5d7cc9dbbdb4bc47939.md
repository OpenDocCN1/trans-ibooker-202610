This is the same window in which you can add groups to grant or deny access to anyone associated with a known group. Groups are better utilized within a networked environment; therefore, you will not set or test group permissions in this lab.

Notice that the user TestUser2 is now included in the ACL. Here you can fine-tune the permissions for this user on the file you created. The default permissions given are to allow Read and Read & Execute.

+   6. Under the Deny column head, click the Write check box. Figure 8.22 shows the Permissions allocated to TestUser2.

![](bbox=[0.203, 0.319, 0.573, 0.648])

**FIGURE 8.22** Permissions for Test1 Given to TestUser2

+   7. Click the Apply button. Read the Windows Security prompt, and then click Yes. You are directed back to the test1 Properties window. Click OK to exit this window.

In regard to permissions, there are a number of rules that apply:

+   ➤ *Deny permissions* take precedence over *allow permissions* in most scenarios. When a deny permission is applied to an object, in this case a `.txt` document, it is called an *explicit deny*.