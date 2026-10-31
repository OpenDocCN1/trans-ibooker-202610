I want to instruct this GPO that the P: drive should only be mapped for users who are part of the Acct Group security group. Choosing to add a new security group to my ILT, I can define the Acct Group and you can see in Figure 5.14 that my P: drive ILT settings have now been updated so that only if the user is a member of the CONTOSO\Acct Group security group, will that user receive the P: drive mapping when they log in:

![](bbox=[0.15, 0.2, 0.85, 0.52])

Figure 5.14: Applying the P: drive mapping to CONTOSO\Acct Group

Repeat this process with the rest of the drive letters inside your GPO, and you now have a single GPO containing all drive letter mappings for your entire organization. You can link this GPO to the domain, leave it security filtered to all authenticated users, as it is by default, and yet based on ILT, only drive letters that correspond to their appropriate groups will now be mapped for each user who logs in. ILT is the bomb!

# Delegation

Consider this scenario: the executive leadership at your company has asked you to lock down security restrictions on all computers in the domain. Sounds like a job for Group Policy! Except, since they are the leadership and don't want their own systems to hinder their ability to browse Instagram and configure desktop backgrounds of smiling kittens, they have required that these lockdown settings apply to everyone except members of the Leadership group in Active Directory.