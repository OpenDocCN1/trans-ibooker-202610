Obviously, there's a better way to handle this situation. One of the very common chores we task to a GPO is the standardized creation of mapped network drives on client computers. Inside a new GPO we can define UNC paths for shares and assign drive letters to them. We can then assign that GPO to users and computers, and drive letters will magically map when your users log into their computers.

Create a new GPO for this purpose, and edit that GPO, as you already know how to do. This time we are navigating to the following location:

# User Configuration | Preferences | Windows Settings | Drive Maps

Right-clicking on Drive Maps and choosing New | Mapped Drive brings you into the configuration section for a single mapped drive letter. You can see in the screenshot below that I am mapping a drive to \\DC1\HR, and assigning it a drive letter of H:

![](bbox=[0.13, 0.357, 0.87, 0.842])

Figure 5.4: Mapping drives with Group Policy