
button, and the Find Now button. Double-click the Domain Admins group and click the OK button. Put a check in the Full Control checkbox in the Allow column.

- e. Click the OK button to close the Permissions window. Click the OK button to close the Advanced Sharing window. Click the Close button to close the Properties window.

6c

## Step 6 Access the share remotely with the new permissions.

- a. You should still be signed in to the domain through the Windows 10 VM as the admin user. This time, you’ll access the shared folder in a different way.

  Search for and select This PC and then double-click Network from the left pane. In the Computer section, in the right pane, you should see an icon and name of your Windows Server 2019 VM. Double-click it. You’ll see your shared folder, in addition to two default shared folders. The sysvol shared folder is used to deliver and store policy scripts and logon scripts to domain members. If there are logon scripts, they would be stored in the netlogon shared folder. Interestingly, according to https://social.technet.microsoft.com/wiki/contents/articles/8548.active-directory-sysvol-and-netlogon.aspx, the netlogon shared folder is not actually a folder named netlogon on the domain controller, but rather just a folder where all the logon scripts are stored.

  If you don’t see your Windows Server 2019 VM from the Windows 10 VM Network applet, try opening the Network applet from the Windows Server 2019 machine. You will be prompted to turn Network Discovery on. Follow the multiple prompts. After that, the icon should be visible from the Windows 10 VM in the Network applet.

- b. Double-click the SecurityPlus shared folder.

- c. Try adding, deleting, renaming, or modifying anything in the shared folder. Now, remotely, those actions work because the user account is a member of the Domain Admins group, which was just granted the

Through a domain policy, you can configure multiple password settings (for example, how often users must change their passwords, password length, the number of unique passwords that must be used before a user can reuse one, and password complexity).

Right-click Default Domain Policy (which will be right under your domain) and select Edit. Put a check in the dialog box that pops up, and select OK to not get the prompt again.

- d. GPO settings fall into one of two initial categories: Computer Configuration and User Configuration. Computer Configuration and User Configuration can each be broken down further into one of two categories: Policies and Preferences. Policies can be broken down further into three categories (click the arrows in the left pane to expand): Software Settings, Windows Settings, and Administrative Templates. Preferences can be broken down into Windows Settings and Control Panel Settings.

  In the Group Policy Management Editor (GPME) window that opens, in the left pane, expand Computer Configuration, Policies, Windows Settings, Security Settings, Account Policies, and Password Policy. Navigating can also be accomplished by double-clicking the same selections in the right pane. Select Password Policy in the left pane to display the various policies and their settings in the right pane.

- e. In the right pane, one at a time, double-click each policy, including Enforce Password History, Maximum Password Age, Minimum Password Age, Minimum Password Length, Password Must Meet Complexity Requirements (this is why your passwords were required to be complex all chapter long), and Store Passwords Using Reversible Encryption. Explore the Security Policy Setting and Explain tabs for each policy.

- f. In the right pane, double-click the Minimum Password Length setting.

  In the Security Policy Setting dialog box, make sure the box labeled Define This Policy Setting is checked. Increase the Password Must Be At Least value to eight characters.

A GPO applied to an OU with differing settings would override.

3e, 3g, 3k

## Step 3 Display a logon warning message with a user configuration GPO.

- a. In the GPMC, right-click the Clients OU in the Canandaigua OU, and select Create a GPO in This Domain, and Link It Here.

- b. In the Name: textbox, type **Logon Warning** and click the OK button.

- c. Expand the Clients OU, right-click the GPO, and select Edit.

- d. In the GPME that opens, in the left pane, expand the following: Computer Configuration, Policies, Windows Settings, Security Settings, Local Policies, and Security Options. Select Security Options. In the right pane, scroll down and double-click Interactive Logon: Message Text For Users Attempting To Log On. Read the Explain tab.

- e. Click the box Define This Policy Setting In The Template. In the textbox, type **This system is restricted to authorized users. Individuals who attempt unauthorized access will be prosecuted. If you are unauthorized, terminate access now. Click OK to indicate your acceptance of this information.** Then click the OK button.

- f. Double-click Interactive Logon: Message Title For Users Attempting To Log On. Read the Explain tab.

- g. Click the box Define This Policy Setting In The Template. In the textbox, type **WARNING: This system is restricted to authorized users.** Then click the OK button.

- h. In the Group Policy Management Console, in the left pane, click the Logon Warning GPO in the Clients OU. In the right pane, click the Settings tab at the top. Click the show links at the right to see information related to this GPO, specifically the show for Interactive Logon.

- i. On the Windows 10 VM, execute **gpupdate**.

- j. Sign out.

- k. Press any key, and you should see the message text and message title configured in your GPO, as shown in Figure 14-7. Click the OK button and sign in as the non-admin user.

![](img/6e921d997751f0a0f87e5170ac9657a1_486_0.png)

**FIGURE 14-7 Logon message**

The Message Text and Message Title policies are used for legal purposes. They warn users (malicious or uninformed ones) about what might happen to them if they misuse organizational resources or information. It also lets them know there could be accounting and auditing of their actions, helping reinforce written policies that users should have read. The Message Text and Message Title policies are also used for cybercriminals. No, an attacker won’t say, “Oh shucks, I can’t hack this one,” and move on to another target. However, studies have shown a higher rate of successful prosecution with these warning messages. With this in mind, it’s a good idea to have warning messages approved by a legal department and human resources.

4e, 4i

## Step 4 The GPOs made so far were immediately linked as they were created. Now you’ll be able to see that the GPOs are in fact objects that can be created and linked in separate steps.

Hide and disable all items on the desktop with a user configuration GPO.

- a. In the GPMC, in the left pane, expand the Group Policy Objects

You will still be able to read and write from that folder, but nothing will appear on the desktop of the system.

5i, 5j

## Step 5 Before getting to the next GPO, you’ll need to change a setting to get Internet access. Then, you’ll download a browser. Finally, you’ll be able to create a user configuration GPO that uses a specific image for the desktop wallpaper. It’s common for an organization to present a uniform look to their machines, and the following steps take you through how it’s done.

- a. From Server Manager, click Local Server in the left pane. In the Properties section at the top, in the right column, click On next to IE Enhanced Security Configuration. Select the radio buttons for Off in the Administrators and Users sections and then click the OK button.

- b. Since Internet Explorer is the only browser on the system by default and is “end of life” (EOL, not supported in terms of updates or security patches by Microsoft anymore), download and install another browser, like Chrome (www.google.com/chrome/) or Firefox (www.mozilla.org/en-US/firefox/windows/).

- c. Using the new browser, perform a Google search for Weissman Security+. Click the Images tab at the top to find the cover of this book. Click one of the thumbnails, and the regular-sized image will appear on the right. Right-click the image, select Save Image As... Then select This PC, C:, and the SecurityPlus shared folder. Change the filename to bookcover.jpg and click the Save button.

- d. In the GPMC, right-click the domain and select Create a GPO in This Domain, and Link It Here.

- e. In the Name: textbox, type Desktop Wallpaper and click the OK button.

- f. Right-click the Desktop Wallpaper GPO, beneath the domain container, and select Edit.

- g. In the GPME that opens, in the left pane, expand the following: User Configuration, Policies, Administrative Templates, Desktop. Then select Desktop (this Desktop is nested inside the previous Desktop).
