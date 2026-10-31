We will cover the following topics in this chapter:

+   - Why use Server Core?
+   - Interfacing with Server Core
+   - Windows Admin Center for managing Server Core
+   - The Sconfig utility
+   - Roles available in Server Core
+   - Building a Server Core domain controller
+   - What happened to Nano Server?

# Why use Server Core?

Why am I even talking about Server Core? Hasn't it been around since 2008? Yes, that is exactly why I am talking about it. The Server Core variant of the Windows Server operating system has been around for quite some time, but it seems like many admins are scared to trust it. I work with many different companies from many different industries. They all have one big thing in common: they use a lot of Windows Servers, and all of these Windows Servers are running the full GUI (Desktop Experience). Have they heard of Server Core? Sure. Have they tested it out in a lab? Sometimes. Everyone seems to have a slightly different experience level with Core, but it's quite rare to find one in production. Maybe I'm just talking to the wrong people, but I have to assume that there is a majority of us out there, myself included, who need to start using Server Core on a more regular basis.

Why do we need to start using Server Core? Because GUI-less servers are the future, says Microsoft. Would you believe that early in the previews for Windows Server 2016, the Desktop Experience option didn't even exist? You couldn't run a full GUI desktop shell on Server 2016 even if you wanted to (which everyone did), other than a mini-shell that could be plopped on top of Server Core. Microsoft received so much flack about this that the full Desktop Experience was added back during one of the Technical Preview rollouts. Even so, since that time, you have probably noticed that Server Core is the default option when installing any Windows Server operating system. Remember, back at the beginning of our book, where we did a quick review of the actual Server 2019 installation? The default option for installation is not Desktop Experience; rather, that top option in Figure 10.1 is the option for installing the command-line-driven Server Core: