
## Group Policy

The next time users inside this OU log into a domain-joined computer, their network drives should be mapped for them during the login process. Logging into my Windows 10 workstation, I now find that to be true and running gpresult again proves that my GPO has successfully been applied!

![](img/d2d47a2ce8c1af61acefc2cdd6d9df70_222_0.png)

Figure 5.10: GPO has been successfully implemented

You can link a GPO to more than one OU. Just follow the same process again, this time choosing a different OU to make the link, and that GPO will now apply to both OUs that have active links. You can remove links by right-click on them and deleting them, or by clicking on the Group Policy Object itself to view and modify its link properties.

## Group Policy processing order

In the GPO linking example that was just completed, we linked a GPO to a specific OU. You probably noticed in some of the screenshots that there are GPOs linked at different levels too, like right at the root of my contoso.local domain. What's that all about? As it turns out, when logging into a computer, there are actually four different levels of Group Policy processing that happens. The location of your links can make a big difference in the proper effect your GPO has on your computers and users. Let's discuss these four levels of GPO processing.

### Local Policy

Assuming you have worked in IT for a little while, it is very likely that you have followed a blog post or forum that has led you into a modification via `gpedit.msc`. Running `gpedit.msc` on any Windows system brings you into the **Local Group Policy Editor**. This is the set of policy objects and settings that exist on an individual machine. They can be manipulated manually via `gpedit.msc`, or they can be manipulated and overwritten by GPOs. The key point that I want to make is that when your Windows computer boots and logs in, the very first thing that happens is that settings inside the Local Group Policy Editor are processed. Since Local Policy is first to apply, it means that any levels of Active Directory Group Policy, which we are about to discuss, will take priority over Local Policy. In other words, your computer may have Local Policy settings in place, but milliseconds later during the boot process, those local settings could be overwritten by AD policy settings.

### Site-level policies

Remember back in *Chapter 3, Active Directory*, when we discussed Active Directory Sites and Services? That comes into play here. If your environment is large enough to contain multiple sites, it is possible to link GPOs at an individual site level, thus enabling Group Policy to issue settings to computers or users based on which site they reside within.

Having GPOs linked to sites is quite a rare occurrence in my experience, but it is something to keep in the back of your mind when troubleshooting GPO applications. Computers and users will only receive site-level policies when they physically reside within those sites, based on the IP addressing scheme and subnets you defined inside AD Sites and Services. If a computer is handed GPO settings based on a site-level link, and these settings contradict what is in Local Group Policy, the site-level policies will override the Local Policy, and these will be the new settings as you continue the login process.

### Domain-level policies

Some policies and settings are going to be things that you want to apply to all machines or users in an entire domain, and the appropriate place for those settings are domain-level GPOs. It's important to point out that the GPOs themselves are not different as we talk about all these different policy levels – a GPO is a GPO. The level at which the GPO is *linked* is what we are discussing in relation to these hierarchical levels.

Links created at the root, or top level, of a domain inside Group Policy Management, will by default attempt to apply to any user or computer that is part of the domain. There are many other factors that can filter domain-linked GPOs, but for the most part if you link a GPO to the domain name, you had better be okay with the settings inside that GPO applying to potentially every workstation, every server, every user. If you peek back a few pages at some of the screenshots from inside the Group Policy Management Console, you can see that my IPAM GPOs are all linked directly to contoso.local. These are known as domain-level links.

Pretending that we are still flowing through a computer login process with these examples, domain-level policies apply after site-level policies. So any settings that came down to you via site-level policies have now been added to, or potentially overridden by (if there was a conflict), our domain-level policies.

### OU-level policies

We started our login journey with Local Group Policy settings, which were then added to or overwritten by site-level GPOs, and those were then added onto again (or overwritten again in the case of conflicts) by domain-level GPOs. You can guess where this is heading. OU-level policies now apply on top of domain-level GPOs.

OUs, as you know, are containing folders for computer and user accounts that are joined to your domain. Most companies take good advantage of using multiple OUs inside Active Directory to differentiate types of machines and users. Servers are separated from workstations, accounting users are different than HR users, and so on. Nesting OUs is a common practice as well. Just like creating folders inside of other folders by using File Explorer, you can use AD Users and Computers to create OUs inside other OUs. This is important to the organization of your domain objects, and it is also important to Group Policy.

Linking GPOs to particular OUs gives us flexibility in handing different settings to different groups of people or machines. When a GPO is linked to a single OU, only computers or users inside that OU, or downstream from it, will be affected by the GPO. You can even have multiple GPOs linked to the same OU.

Nested OUs provide an additional tier to this GPO workflow. Remember the general rule is that Group Policy processes from the top down, so GPOs that are linked to a nested OU will most likely outweigh GPOs that are linked at a higher-level OU.

## Security Filtering

Now that you have created a GPO and linked it to particular OU, you have enough information to start using Group Policy in your environment. Using links to determine what machines or users get what policies is the most common method that I see admins use, but there are many circumstances where you might want to take it a step further. What if you had a new GPO and had it linked to an OU that contained all of your desktop computers, but then decided that some of those machines needed the policy and some did not? It would be a headache to have to split those machines up into two separate OUs just for the purpose of this policy that you are building. This is where GPO Security Filtering comes into play.

Security Filtering is the ability to filter a GPO down to particular Active Directory objects. On any given GPO in your directory, you can set filters so that the GPO only applies to particular users, particular computers, or even particular groups of users or computers. I find that using groups is especially useful.

For our Map Network Drives GPO, right now I have it linked to my Accounting Users OU, but what if I wanted to filter that GPO in a different way? Rather than limit the scope of this GPO to a single OU, I would prefer to make it a little bit further reaching, and I'm going to link the GPO to my contoso.local domain. Yikes, doesn't that mean it's going to apply to everything? Yes, unless you utilize the Security Filtering section to define user accounts or groups of accounts to which it should apply.

> I always make my Security Filtering decisions before creating a link. Links are live as soon as you create them, so building Security Filtering first ensures proper distribution from the start.
