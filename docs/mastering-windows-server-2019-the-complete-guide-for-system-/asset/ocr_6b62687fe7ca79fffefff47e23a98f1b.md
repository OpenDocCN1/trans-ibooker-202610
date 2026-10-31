# MMC and MSC shortcuts

You have probably noticed that many of the management consoles that we utilize to configure components inside Windows Server 2019 look pretty similar. What happens under the hood with a number of these consoles is that you are actually looking at a snap-in function, a specific set of tools that are snapped into a generic console tool called the **Microsoft Management Console**, more commonly referred to as **MMC**. In fact, rather than opening all these management functions from inside Server Manager, for many of them, you could simply type MMC by navigating to **Start** | **Run** or Command Prompt and invoke the generic MMC console. From here, you can click on the **File** menu and choose **Add or Remove Snap-ins**:

![](bbox=[0.13, 0.295, 0.87, 0.715])

Figure 15.27: Using MMC to snap in management consoles