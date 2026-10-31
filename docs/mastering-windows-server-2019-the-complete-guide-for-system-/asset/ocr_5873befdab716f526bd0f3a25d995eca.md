Referencing the same Map Network Drives GPO that I have been working with for many pages now, you know that this policy contains multiple drive letters. Currently, my Acct Group users receive all of those mapped drives. Let's pretend for a minute that I only want the Acct Group to receive one of those drive letter mappings, and not the others. Editing the GPO, I double-click on my P: drive mapping, and then visit the **Common** tab. Here, I check the box for **Item-level targeting** and click on that **Targeting...** button.

Inside the Targeting Editor, choose **New Item** and you will find a large list of criteria that can be utilized for these additional targeting parameters:

![](bbox=[0.13, 0.27, 0.87, 0.74])

Figure 5.13: Adding a new security group in Targeting Editor