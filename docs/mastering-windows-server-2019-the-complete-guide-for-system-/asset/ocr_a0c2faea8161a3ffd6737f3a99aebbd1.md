Interestingly, the Command Prompt window isn't consuming the full screen; it is clear that there is a black background that cmd.exe is riding on top of. I only find this interesting because you can tell that the Core operating system itself is something other than Command Prompt, and that cmd.exe is just an application that autolaunches upon login. You can even utilize the mouse here and resize or move that Command Prompt window around. I do wonder if and when this will be replaced with a PowerShell prompt as the default interface.

Even more interesting and good to know is that you can launch some GUI-like applications from this prompt. For example, you can open up Notepad and utilize it with both a keyboard and mouse, just like you would from any version of Windows. If you have Notepad open, create a note and then save it; you can see that there is a real file structure and a set of relatively normal-looking system folders. So, rather than some form of black magic, Server Core is actually the real Windows Server operating system, wrapped up in a smaller and more secure package:

![](bbox=[0.13, 0.37, 0.87, 0.72])

Figure 10.5: Opening Notepad on Server Core