
## Building a GPO

There's nothing quite as good as jumping in and getting your hands dirty, so let's get down to business and build a new GPO. Don't worry, we will be careful not to apply this GPO to anything yet and save that for our next section. As with most Microsoft technologies, there is a special management console created just for interacting with Group Policy, appropriately named the Group Policy Management Console (GPMC). Logging into any of your domain controller servers, you can launch GPMC from inside Administrative Tools, inside the Tools menu of Server Manager, or by launching GPMC.MSC from Start | Run, Command Prompt, or PowerShell:

![](img/d2d47a2ce8c1af61acefc2cdd6d9df70_210_0.png)

Figure 5.1: GPMC

You'll notice in *Figure 5.1* that there are already some GPOs listed here. They are a combination of default GPOs that always exist when you install Active Directory (we'll talk about the Default Domain Policy a little later in this chapter) and the IPAM GPOs that the IPAM configuration process put into place for us in *Chapter 4, DNS and DHCP*. To create a new GPO in a way that it is not yet applying to any workstations or users, right-click on the **Group Policy Objects** folder and select **New**. Create a name for your new GPO, click **OK**, and you have created a Group Policy Object! So far, your new GPO is void of any settings or configurations, and it is not applying to anything or anybody, so it is precisely pointless. We will soon change that...

## Adding Trusted Sites

I named my first GPO "Adding Trusted Sites" because I am going to use this new GPO to apply some URLs to be recognized as trusted sites inside Internet Explorer on my Windows 10 client computer. If you run a web application in your network that needs to run JavaScript or ActiveX controls, or something like that, it may be required that the website is part of the trusted sites list inside Internet Explorer for it to run properly. You could print off an instructions page for the helpdesk on how to do this on each computer and make them spend the time to do it for every user who calls in because they cannot access the application. Or you could simply create a GPO that makes these changes for you automatically on every workstation and save yourself from dealing with all of those phone calls. This is just one tiny example of the power that Group Policy possesses, but it's a good example because it is useful, and it is a setting that is buried way down in the GPO settings, so you can get a feel for just how deep these capabilities go.

## Group Policy

Right-click on the new GPO and choose Edit... Now navigate to Computer Configuration | Policies | Administrative Templates | Windows Components | Internet Explorer | Internet Control Panel | Security Page. See, I told you it was buried in there!

![](img/d2d47a2ce8c1af61acefc2cdd6d9df70_212_0.png)

Figure 5.2: Creating a GPO to manage trusted websites

Now double-click on Site to Zone Assignment List and set it to Enabled. This allows you to click on the Show... button, within which you can enter websites and give them zone assignments. Each GPO setting has a nice descriptive text to accompany it, telling you exactly what that particular setting is for and what the options mean. As you can see in the text for this one, in order to set my websites to be trusted sites, I need to give them a zone assignment value of 2. And, just for fun, I also added a site that I do not want to be accessible to my users and gave it a zone value of 4 so that badsite.contoso.com is a member of the restricted sites zone on all of my desktop computers. Here is my completed list:

![](img/d2d47a2ce8c1af61acefc2cdd6d9df70_213_0.png)

Figure 5.3: Assigning websites to different zones

Are we done? Almost. As soon as I click on the OK button, these settings are now stored in my Group Policy Object and are ready to be deployed. As you know, we have not assigned this new GPO to apply to anybody yet, so for now the GPO is populated with these settings but still doing nothing. Before we push out these settings, let's build out a couple more common GPOs to make sure our examples here are well-rounded.

## Mapping network drives

File servers are some of the most common types of servers that exist because all companies across all industries need to create and maintain documentation to run their businesses. This is not a chapter about how to build a file server, set up shares, restrict permissions, or utilize Distributed File System (DFS) to improve the overall flexibility and resiliency of your file server infrastructure, although these are all good things to learn. Today, we are going to assume that you already have file servers in place and that there are already shared folders on those file servers. In my test lab, I have shared folders from a few different servers, as identified here:

- \\DC1\HR
- \\DC2\Accounting
- \\WEB3\Installers

The challenge we are trying to solve is how to automate these shared folder locations being available on all of my user's workstations. I could put together a piece of documentation that shows users how to manually get to these locations by using UNC paths plugged into the address bar of File Explorer. Or maybe even take it a step further and show my folks how to map network drives from inside File Explorer so that they end up with drive letters assigned on their computers for ongoing access to these locations. Doing so would work, but it puts an administrative burden on my users, and also leads to the possibility of users having differing drive letters. Grace might use her "R" drive letter to map to the accounting share, while Jackson may decide that "T" is his accounting drive letter of choice.

Obviously, there's a better way to handle this situation. One of the very common chores we task to a GPO is the standardized creation of mapped network drives on client computers. Inside a new GPO we can define UNC paths for shares and assign drive letters to them. We can then assign that GPO to users and computers, and drive letters will magically map when your users log into their computers.

Create a new GPO for this purpose, and edit that GPO, as you already know how to do. This time we are navigating to the following location:

### User Configuration | Preferences | Windows Settings | Drive Maps

Right-clicking on Drive Maps and choosing New | Mapped Drive brings you into the configuration section for a single mapped drive letter. You can see in the screenshot below that I am mapping a drive to \\DC1\HR, and assigning it a drive letter of H:

![](img/d2d47a2ce8c1af61acefc2cdd6d9df70_214_0.png)

Figure 5.4: Mapping drives with Group Policy

You'll notice the **Action** dropdown menu has four different options: **Create**, **Replace**, **Update**, and **Delete**. This is important to understand as many GPO preference configurations have this same drop-down selection. Here is a quick summary of each available option as it relates to our new drive mapping policy:

- **Create**: Use this action to create the new mapped drive only if it does not already exist. If the H: drive is already in use on a workstation, this new mapping will then be ignored. In our example, if I were to configure this new drive mapping for Create, it would only take an action if the H: drive letter was currently open and available.
- **Replace**: Use this action to remove an existing setting and replace it with a new setting. In our case, it will update whatever mapped drive is using H: to our \\DC1\HR. The Replace option is redundant because of the Update option and is rarely used.
- **Update**: This is the default action for most preference settings and is generally the most useful. If the setting that we are configuring doesn't exist, Update will create it. Additionally, if the setting (mapped drive) is already in place on the workstation, it will now be updated to reflect our new definition inside the GPO. Drive mapping policies almost always use the Update action to push new drive letters into place.
- **Delete**: This removes the specific setting from the client machine. If you are removing a network share and want to ensure that it is removed from all of the computers in your domain, this would be a useful Action to ensure that happens.
