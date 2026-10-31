Depending on what stats and hardware you are interested in monitoring, select the appropriate capability and click the **Invoke** button, or select them all just to see what System Insights comes back with. As you can see in *Figure 15.22*, I invoked all four, and currently, they are all reporting green. System Insights has determined that these components of my WEB1 virtual server are in good shape and are forecasted to remain that way:

![](bbox=[0.13, 0.205, 0.87, 0.405])

Figure 15.22: System Insights data displayed inside Windows Admin Center

# Remote toolsets

Here is another section where you already have the information you need if you have been working through this book from start to finish, but it never hurts to receive a gentle reminder on taking what you have learned and putting it into practice.

Remote Desktop, Hyper-V Console, PowerShell, Sconfig, Windows Admin Center, MMC consoles, Server Manager. These are all different tools, any of which could be used to accomplish similar tasks on your servers. I love training new IT staff into our service desk and working alongside them on tickets, especially in areas where they have limited experience and are hesitant to dive in. Using the Remote Desktop client to RDP into servers is still by far the most common way that administrators log into their servers to make changes or for any reason, and when you do that all day every day, it is easy to forget about all the other ways that you can interact with those servers. RDP is one of the first things that will quit working when a server is under heavy load or struggling to breathe.